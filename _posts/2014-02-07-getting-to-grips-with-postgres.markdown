---
layout: post
title: "Getting to grips with postgres"
date: 2014-02-07 12:10:20 +0000
comments: true
categories: [ruby, rails]
tags: [ruby, rails, postgres, soldn]
---

So, having generated my Rails app and selected postgres as the development database, I'm now faced with getting the damn thing running. On a Eurostar train (under the English channel) i.e. with no internet.

In the process I learned some things about using documentation without an internet connection, and about postgres.

TL;DR
-------------
Steps to get postgres working with Rails on OSX:

  * Install postgres with homebrew: `brew install postgres`
  * Start the postgres server according to the instructions in `brew info postgres`
  * Create a database user with `createuser [username]` - making sure to give it database creation privelleges
  * Run rake db:create

Why postgres?
-------------
Previously I'd always just used sqlite for development (which is pretty much zero-configuration) and at work we historically used MySQL because the ops guys are mostly supporting WordPress sites (MySQL is the default database for WordPress). The current version of Swing Out London uses postgres in production(Heroku), but if I recall correctly heroku handled all the setup, so I've never really had to work with postgres before.

For SOLDN2 I want to start using postgres locally as well as on production to try and avoid any issues e.g. when copying the prod database locally.

Running the postgres server
----------------------------
I'm using Zeus, so the first thing I tried was starting up the rails server (`zeus s`). I got some sort of error about not being able to connect to the database

I'd previously installed postgres using homebrew (`brew install postgres`), but it seems I hadn't actually set it up properly, so there was no server running. `brew info postgres` told me I needed to run a few commands:

    # Create a database running with a user named after my system user:
    % initdb /usr/local/var/postgres -E utf8

    # Run the database server in the background
    % pg_ctl -D /usr/local/var/postgres -l logfile start

    # Set up the postgres server to start at login:
    % ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents

So that created a database with a user of duncanstuart - not sure if that's right. Is that like a box for my application DB's to go in, or is it a single database in itself?

Database config
---------------
So now on restarting zeus and running the rails sever I get this error:

    PG::ConnectionBad at /
    FATAL:  role "myapp" does not exist

Looking at my `database.yml`, it seemed that the application generator had set up several 'myapp' databases:

{% highlight database.yml %}
development:
  adapter:  postgresql
  host:     localhost
  encoding: unicode
  database: myapp_development
  pool:     5
  username: myapp
  password:
  template: template0

  ...
{% endhighlight %}

Replacing myapp with swingoutlondon, I dared to hope that running `rake db:create` would just create these databases for me...

Getting at the docs
-------------------
No such luck:

    FATAL:  role "swingoutlondon" does not exist

I haven't come across the term 'role' before, but given that it's from the username field in the database.yml, I guess that's what postgres calls its db users? Somehow I need to create that role manually.

Needing some documentation: how do you get a console up in postgres? for MySQL it's just `mysql`, but just `postgres` (or actually postgres -D /usr/local/var/postgres) seems to be the command for starting the server.

`postgres --help` wasn't much help. In the install directory for postgres (in my case `/usr/local/Cellar/postgresql/9.1.2/`) there's a `share` directory contain man and lib. `lib postgres` wasn't much help either, but looking at `/usr/local/Cellar/postgresql/9.1.2/share/doc/postgresql/html/index.html` in my browser brings up an html view of some long-form docs. Now we're cooking.

Postgres console
-----------------
The postgres console command is `psql`:

    psql: FATAL:  database "duncanstuart" does not exist

OK, so I guess the default looks for a database named after my system user, and I guess this means I hadn't created a database in the steps above. So let's create a dev database:

    % createdb swingoutlondon_development
    % psql swingoutlondon_development

Bingo! I'm in the (pretty odd looking postgres console). `\h` brings up a list of commands. Let's create the role:

    > CREATE ROLE swingoutlondon;

That didn't return any output. Looking at the list of commands I can't see how to list the roles, but running the command a second time complains that the role already exists, so I guess it's OK.

Actually, screw the postgres console
------------------------------------
Looking back at the html docs to find out more about roles it seems like a better way to do this is from the command line:

    % createuser swingoutlondon

This gives me a few options;

    Shall the new role be a superuser? (y/n)
    #=> n
    Shall the new role be allowed to create databases? (y/n) y
    #=> y
    Shall the new role be allowed to create more new roles? (y/n) n
    #=> n

Not sure about those options, but my rationale is, there's no need for the user to have any special privelliges, but perhaps if I let it create databases then my `rake db:create` will work(?).

...and indeed it does! Restarting zeus I get the familiar Rails "Welcome Aboard" page


