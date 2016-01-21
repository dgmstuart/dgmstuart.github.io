---
layout: post
title: "Gotcha: Initialising RSpec in a rails project"
date: 2014-02-10 07:50:09 +0000
comments: true
categories: [Ruby, RSpec, Rails, Testing]
---
I use RSpec for doing TDD, and the rspec-rails gem for integration in Rails projects. On initialising RSpec for SOLDN2, with `rspec init` it generated the following spec helper (comments removed):

{% codeblock spec/spec_helper.rb %}
RSpec.configure do |config|
  config.treat_symbols_as_metadata_keys_with_true_values = true
  config.run_all_when_everything_filtered = true
  config.filter_run :focus
  config.order = 'random'
end
{% endcodeblock %}

This seemed to be missing some things, notably the loading of the Rails environment. Looking at the readme of the rspec-rails gem, I found that the correct command is actually:

    rails generate rspec:install

The file this command generates is much more verbose with instructive comments:

{% codeblock spec/spec_helper.rb %}
ENV["RAILS_ENV"] ||= 'test'
require File.expand_path("../../config/environment", __FILE__)
require 'rspec/rails'
require 'rspec/autorun'

Dir[Rails.root.join("spec/support/**/*.rb")].each { |f| require f }

ActiveRecord::Migration.check_pending! if defined?(ActiveRecord::Migration)

RSpec.configure do |config|
  config.fixture_path = "#{::Rails.root}/spec/fixtures"
  config.use_transactional_fixtures = true
  config.infer_base_class_for_anonymous_controllers = false
  config.order = "random"
end
{% endcodeblock %}

Interestingly this leaves out the `focus` config options, which allow you to select specific specs to run, e.g:

```ruby
it "does a thing", :focus do
  #...
end
```
