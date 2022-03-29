---
layout:    default
tags:      developement rails 2cent
title:     rails cheat sheet, things to remember
---
### rails cheat sheet

I seldom start new rails application, but when the exception happens I often end up with a 'task' and the knowledge that there is a better way to handle the command line, but I can't remember!

Therefore I note a this **wiki wiki page** some **plain and ordinarily commands, don't expect fancy tricks** as it is more of a **mnemonic** for me. For details check `rails --help` or `rails generate --help`!

#### basic setup

    $ rails new cool-app
    $ cd cool-app
    $ rails server

Install [RSpec][1]

    $ echo "gem 'rspec-rails'" > Gemfile
    $ bundle install
    $ rails generate rspec:install

and generate a simple user application with specs:

    $ rails g scaffold User name:string birthday:date

#### bundle

Find outdated gems with `bundle outdated` and update installed ones with `bundle update`.

#### rspec

Generate default rspec test for existing controller 'events_controller.rb'

    $ rails g rspec:controller Events

#### sporc

tbd!

#### update

Update Ruby and Rails version in the Gemfile, and excute update script

    $ bundle update --bundler
    $ bundle update
    $ export THOR_MERGE=meld && bin/rails app:update
    $ bin/rails db:drop
    $ bin/rails db:test:prepare
    $ bin/rails db:migrate
    $ bundle exec guard

After this, check Rails upgrade guide at https://guides.rubyonrails.org/upgrading_ruby_on_rails.html
for more details about upgrading your app.

#### foreman

tbd: Procfile

  [1]: https://relishapp.com/rspec/rspec-rails/docs
