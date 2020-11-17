---
title: Rails Boilerplate
date: 2020-07-07
published: true
tags: ['rails', 'ruby', 'webdev', 'tutorials']
series: Streamlined Workflow
cover_image: images/rails.jpg
canonical_rul: false
description: So far in this series, we have looked at the benefits of using a Boilerplate to streamline your workflow, and a two articles on how to setup a React Boilerplate with parcel instead of Webpack, and a traditional means to compile and bundle a traditional HTML/SCSS project using Parcel. So, what about Rails?
---
So far in this series, we have looked at the benefits of using a Boilerplate to streamline your workflow, and a two articles on how to setup a React Boilerplate with parcel instead of Webpack, and a traditional means to compile and bundle a traditional HTML/SCSS project using Parcel. So, what about Rails?

## Config File
If you’ve used Unix/Linux for any length of time, you’re familiar with dotfiles. They’re those files beginning with a `.` and usually ending with rc: `.bashrc`, `.vimrc`, and `.zshrc` being familiar examples. Modifying these files allows you to configure the behavior of the associated program, and can be a never ending quest for some to get the “perfect” setup.

You can streamline new Rails project creation with the use of a `.railsrc` file. There are a few rules:
- The file **must** be named `.railsrc` (don't forget the preceeding `.`
- The file must be placed in the users path. So, it has to be in your `$HOME` directory.

Follow these simple rules, and the `rails new` command will inject commands to the command line silently. For instance, I never use the default Rails database SQlite, I always add the commandline switch to use Postgres. Do I want to type this every time? **NO!!!!!**

So my simple `.railsrc` file:
```
--database=postgresql
```
I can start a new project: `rails new cool_app` and silently the postgresql flag is added to the install process.

What are your defaults? What do you change or use each time? A more involved setup may look like this:
```
--database=postgresql
--webpack
--skip-action-cable
--skip-spring
--skip-turbolinks
```
There is more we can do.

## Template Files
The last line of the `.railsrc` file is reserved for template files that will run during install. So, let's look at my simple `railsrc` file again:
```
--database=postgresql
--template=/path/to/file/rails-template.rb
```
I have stored my simple template file locally, and you can name as you like. Here is my simeple script:
```ruby
#add guard-minitest and spring to dev

gem_group :development do 
  gem 'pry-rails'
  gem 'awesome_print'
end

gem_group :test do
  gem 'guard'
  gem 'guard-minitest'
  gem 'minitest'
  gem 'minitest-reporters'
  gem 'rails-controller-testing'
end

run "bundle install"

generate :controller, "static_pages home"
route "root 'static_pages#home'"

run "cp ~/development/my-docs/starter/renovate.json ."
run "cp ~/development/my-docs/starter/LICENSE ."
run "cp ~/development/my-docs/Guardfile ."

#create postgres DB for postgress.app
run "psql -c 'CREATE DATABASE #{app_path}_development;'"
run "psql -c 'CREATE DATABASE #{app_path}_test;'"
```
In this template I am setting up my beginning project preferences:
- Set up a better console printing with `awesome_print`
- Setup my preferred testing environment
- Bundle install
- Setup a static controller for the Home page
- Copy a few files
- Create the Postgres databases.

So, all I have to do, is change to the new project directory and start the Rails server. The DB is setup enough to get started. This works for me but I want to do more.

### More Resources

[Matt Brictson](https://github.com/mattbrictson/rails-template) has put together a more full featured Rails template that is preloaded with the best practices for TDD, security, deployment, and developer productivity.

**Jumpstart**

Building on the work of Matt's open source template Chris Oliver has put together a template called [Jumpstart](https://github.com/excid3/jumpstart/blob/master/template.rb). This template creates a Boilerplate app that includes user accounts, admin interface, and Bootstrap styling.

**Jumpstart Pro**

Chris also has a commercial product called [JUmpstart Pro](https://jumpstartrails.com/) that bootstraps a full featured e-commerce application setup.

### Post install template

What about automating post-install tasks? Wel,, I highly recommend going to the free service [RailsBytes](https://railsbytes.com/public/templates) and try some templates out.

For instance, do you want to install Rubocop:
```
rails app:template LOCATION='https://railsbytes.com/script/V33sBe'
```
Thats it. Check it out.

## Rails Minimal
Finally, there is a new feature coming to the `rails new` command to create a minimal application. The original [pull request](https://github.com/rails/rails/pull/39282) has been merged, but there appears to be still some documentation pull request in the works. When it is finished and deployed the command `rails new cool_app --minimal` will look like the following:

- skip_action_cable
- skip_action_mailbox
- skip_action_mailer
- skip_action_text
- skip_active_job
- skip_active_storage
- skip_bootsnap
- skip_jbuilder
- skip_spring
- skip_system_tests
- skip_turbolinks
- skip_webpack
- Sprockets should have JavaScript folders by default
- rails new --minimal webpack=react still works

When fully deployed, this will be a nice feature to have for streamlining the workflow.

## Footnote
This has been fun. Leave a comment or send me a DM on [Twitter](http://twitter.com/EclecticCoding).

Shameless Plug: If you work at a great company and you are in the market for a Software Developer with a varied skill set and life experiences, send me a message on [Twitter](http://twitter.com/EclecticCoding) and check out my [LinkedIn](http://www.linkedin.com/in/dev-chuck-smith).
