# Code blog - source

Source code for blog hosted at https://dgmstuart.github.io/.
Blog repo: https://github.com/dgmstuart/dgmstuart.github.io

Based on Jekyll, using Octopress plugins.

### Install dependencies:

    bundle install

### Run local server

    bundle exec jekyll serve

### Deploy

First edit `_deploy.yml`, then:

    jekyll build;octopress deploy

Make sure you don't have a server running at the same time! Otherwise you
might end up pushing a version with localhost urls instead of the production
url.

