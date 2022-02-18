---
layout: post
title: "Some tools for faster blogging in Octopress"
date: 2015-03-27 18:48:31 +0000
comments: true
category: octopress
tags: [blogging, vim]
---

I've been to a number of conferences and talks recently and I've developed a habit
of live-blogging my notes: getting them up online as quickly as possible.

More on that in a future post but I want to talk about how I am able to start taking
notes fast, and publish them even faster (when a talk finishes, lots of people will be
trying to get out of the row of seats past me!).

_NOTE: The scripts below assume editing with terminal vim. It might be more flexible to switch this out for
the `$VISUAL` environment variable (?), but if you use a different editor you'll probably
just want to replace `vim` with whatever command you use to boot up your editor - e.g. `subl`, `mvim` etc._

### Creating posts
The octopress command for creating a post is `rake new_post["The name of my
post"]`. I'd then typically select the filename it outputs with the mouse,
paste that into the command line and open it with vim. That's _way_ too much
typing, and any seasoned vim user would wince at the word "mouse".

Here's a function which lets me type `newpost "The name of my post"` and creates
the post, isolates it and opens it in one step:

{% highlight shell %}
#  ~/.profile

function newpost(){
  cd ${BLOG_HOME:?"Need to set BLOG_HOME to the location of the octopress blog directory"}
  output=$( rake new_post["$@"] )
  echo "$output"

  local return_code=$?
  if [ $return_code -eq 0 ]; then
    # Get the name of the file which was just created:
    post_path=$(grep -o 'source\/_posts\/.*\.markdown' <<< $output)
    # Stash all other posts (for faster generation)
    echo "Isolating post..."
    rake isolate["$post_path"]
    # Open with the cursor at the bottom of the header:
    vim -c 'normal G' $post_path
  fi
}
{% endhighlight %}

My [dotfiles](https://github.com/dgmstuart/dotfiles) are shared between
multiple machines, so the `BLOG_HOME` environment variable is set in a local
profile:

{% highlight shell %}
#  ~/.profile
export BLOG_HOME=$HOME/Dev/Personal/blog
{% endhighlight %}

I'm pretty new at shell scripting, so if you know of a way to make this better
or more robust, please let me know in the comments.

### Isolating Posts

Notice in the `newpost` function I've got `rake isolate["$post_path"]` - that's a
nifty task provided by Octopress which gets around the fact that when you generate
your Octopress blog it has to rebuild the entire site (?) which can be very slow.
Unfortunately it doesn't seem to be documented, but you can see the source in the
`Rakefile` of your Octopress install.

_Side note: run `rake -T` in your blog directory to see all of the Octopress commands #protip_

`rake isolate` moves all posts into `source/_stash`, unless they  match the given
string in their title. This means that when you're previewing your post with `rake preview`
it's much faster to re-generate because it's only having to handle one post each time.

If you're running this manually, there's no need to pass the whole path: you can just
find one of two words which are unique in the filename of your post and use that - e.g.
`rake isolate[wizards]`

You can put your stashed posts back with `rake integrate`.

### Listing and revisiting posts
A couple of things I find myself needing to do fairly often:

* Make an edit to the post I was most recently editing
* Find a post where I can't remember the title was

The following aliases achieve that:

{% highlight shell %}
# ~/.profile

# blogposts: display the list of posts ordered by last modified time
# %m                           -- Last modified timestamp
# %N                           -- Quoted File name
# -exec stat -f "%m %N" {} \;  -- Outputs a timestamp at the beginning of the line for sorting
# cut -d ' ' -f2-              -- Returns only the second field in a space-delimited string
alias blogposts='find $BLOG_HOME/source/_posts/* -exec stat -f "%m %N" {} \; | sort -n | cut -d " " -f2-'
alias lastpost='find `blogposts` | tail -1 '
alias epost='vim `lastpost`'
{% endhighlight %}

### Deploying
The `gen:deploy` rake task is a built-in alias for running the
`:integrate`, `:generate`, and `:deploy` tasks, but again, typing
`rake gen:deploy` is just too much typing, hence:

{% highlight shell %}
# ~/.profile

alias rgd='rake gen_deploy'
{% endhighlight %}
