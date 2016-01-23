---
layout: post
title: "Migrating Postgres data from an old Rails project to a new one"
date: 2014-08-20 23:29:31 +0100
comments: true
categories: [ruby, rails]
tags: [ruby, rails, postgres, heroku]
---
TL;DR - this was my approach:

1. create migrations on the old project to transform the data into the new schema structure
1. run these migrations on a dump of the live database
1. dump just the data of the tables you care about from the migrated database with pg_dump
1. import this dump into the new database with psql

Once I worked through a few kinks, it was all remarkably straightforward.

Background
----------
* [Swing Out London](https://github.com/dgmstuart/Swing-Out-London) is a cms for managing swing events
* I'm rebuilding it from scratch as [a totally new project](https://github.com/dgmstuart/swingoutlondon2)
* In particular the [old data model](https://github.com/dgmstuart/Swing-Out-London/blob/274f64e1d635bcd8d2678eb6a0dfa50516ef64ba/db/schema.rb) was insane and the [new one](https://github.com/dgmstuart/swingoutlondon2/blob/daa4397f1e9d772a5b5302cdd369b81201c8ec84/db/schema.rb)  is turning out much cleaner
* The current live system is Rails 3 and hosted on Heroku - so uses postgres for the live database, but uses sqlite for the development database
* The new system is Rails 4 and uses postgres for the development database

I want to be able to import data from the current live site into my work-in-progress new schema as I go along, for a few reasons:

2. Doing the migration incrementally will hopefully be easier than doing it all in one go at the end
1. Working with real data will help me make better design decisions earlier
3. Being able to see what is missing between the old and new schemas will help to make sure I don't miss anything

The challenge
--------------

Data which in the old app exists in one god object (`event`) needs to be divided between three tables in the new app (`events`, `event_seeds`, `event_generators`).

In particular, in the new app, events with individual dates are modelled by having:

* an `event_generator` with a date, which belongs to...
* an `event_seed` (which holds the main data) and belongs to...
* an `event` which groups `event_seed`s together.

In the old app, dates are stored in a `swing_dates` table (which has unique entries)
and associated with `events` via a join table: `event_swing_dates` _(Don't do this. If you can believe it, this was done in an attempt to speed up database queries)_.

The thought of transforming this data at the SQL level gives me a headache. Fortunately in the old app this complexity is hidden under the hood and events just have a `#dates` method which pulls out the relevant list. For the sake of sanity, my import solution needs to take advantage of that method - hence doing the transformation at the application level using migrations.


Step 0: Convert the old app to use postgres
--------------------------------------------
I had never gotten around to updating the old app to use posgres in development, so the first step was to sort that out. Luckily this is pretty easy:

1. In the `Gemfile`, remove the `sqlite3` gem and move the `pg` gem out of the production group.
2. In `database.yml`, change the sqlite config to a posgres config

Step 1: Create migrations
--------------------------
I used a number of migrations to do the data transformation. Since some of them are irreversible, there's no real reason to separate them out, other than organising the code:

### ChangeVenuesToNewFormat
The venues table isn't changing much so this just involved renaming a column, adding some not-null constraints (probably not necessary) and dropping the columns which aren't being imported yet ([source](https://github.com/dgmstuart/Swing-Out-London/blob/01df37393444e4d3e217c870d6b3164e001288f0/db/migrate/20140820003740_change_venues_to_new_format.rb))

### CreateNewEventsTables
This involved copying the section of the schema in the new app which creates the tables for the `event_seeds` and `event_generators` tables. This way the tables are exactly the same as the new schema. ([source](https://github.com/dgmstuart/Swing-Out-London/blob/9069b2d1b4103b781a7b3e9951317c40c1d3fb2b/db/migrate/20140820082042_create_new_events_tables.rb))

### MoveDataToNewEventTables
This is where the gnarly stuff happens. First I need a way of putting data into the new tables. I could go and create full models in the app, but it's possible to just create lightweight classes right in the migration:

```ruby
class EventSeed <  ActiveRecord::Base
  belongs_to :event
  belongs_to :venue
end
class EventGenerator < ActiveRecord::Base
  belongs_to :event_seed
end
```
I guess I should maybe also have included all of the validations here, but I'm fairly confident that the validations are basically equivalent between the two apps, and in any case, the relevant fields have `null: false` constraints on them in the database, so I'm not going to sweat it.

Now we can actually create the new records:

```ruby
def change
  Event.all.each do |event|
    event_seed = create_event_seed(event)

    if event.frequency == 1
      start_date = event.first_date || Date.new(2001,1,1)
      create_event_generator(1, start_date, event, event_seed)
    else
      event.dates.each do |date|
        create_event_generator(0, date, event, event_seed) if date > Date.today
      end
    end
  end

  # ...
end
```
Note the use of `event.dates`.

`create_event_seed` and `create_event_generator` are just wrappers around
`EventGenerator.create` and `EventSeed.create` for convenience.

This migration also dropped most of the fields on `events` but kept the original ids intact - I have a feeling that will turn out to be useful later on. ([source](https://github.com/dgmstuart/Swing-Out-London/blob/01df37393444e4d3e217c870d6b3164e001288f0/db/migrate/20140820082444_move_data_to_new_event_tables.rb))

### CreateNonMigrationTables and DropUnusedTables
These migrations create the remaining missing tables (like Users, which we're not going to import data into) and drop the tables which don't yet have any counterpart in the new app. ([source 1](https://github.com/dgmstuart/Swing-Out-London/blob/01df37393444e4d3e217c870d6b3164e001288f0/db/migrate/20140820210352_create_non_migration_tables.rb), [source 2](https://github.com/dgmstuart/Swing-Out-London/blob/01df37393444e4d3e217c870d6b3164e001288f0/db/migrate/20140820210653_drop_unused_tables.rb))

Given that in the end I only migrated data from specific tables, this probably wasn't necessary, but it's handy to be able to verify that the schemas between the old and new app exactly match, and to see what hasn't been migrated yet.

Step 2: Run the migrations on the live database
-----------------------------------------------
So we've now got a set of migrations which convert the old schema into the latest version of the new schema. Time to test it out on some real data.

Getting a database dump from heroku is easy. You first need to drop the existing development database with `dbdrop soldn1_dev` and then you can create a new one as a copy of the live database. For me that looks like this:

    heroku pg:pull HEROKU_POSTGRESQL_GRAY soldn1_dev

Then it's just a case of `rake db:migrate` and cross your fingers. If it's successful then the `schema.rb` files should match between the old and new app (perhaps modulo some indexes).

Step 3: Dump the data
----------------------
So now we've got our data into the form we want it, how do we actually move it into the new database?

Dumping the whole database would mean we'd carry over the `schema_migrations` table as well. `schema_migrations` consists of a list of identifiers which match up to an app's migration files - it's what Rails uses to track which migrations have been run, which is important for e.g. `rake db` commands, so we don't want to modify it.

The simplest approach seems to be to dump only the tables which actually contain data which we want to migrate:

    pg_dump soldn1_dev -a -t event_generators -t event_seeds -t events -t venues > dump

`pg_dump` actually writes to `stdout`, so that `> dump` creates a file from the output.

The `-a` flag tells `pg_dump` to dump only the data. Although we've engineered a state where the migrations on the old site and the new site both result in the same schema, it seems cleaner to only move across the data rather than recreating the tables.

Step 4: Import the data
-----------------------
If there are any records already in the relevant tables with IDs which would clash with data in the import then this would need to be resolved somehow before importing. I don't care about any existing data, so I'm happy to resolve it by clearing out any existing records with `rake db:reset`.

To load the data from the dump file:

    psql soldn2_dev < dump

And that's it: the data is now in the new database. Job done.

_Thanks to [@zeeraw](https://twitter.com/zeeraw) for the suggestion to use Rails migrations_
