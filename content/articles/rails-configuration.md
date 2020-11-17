---
title: Rails Configuration
date: 2020-04-08
published: true
tags: ['Ruby', 'Rails', 'TIL']
series: false
cover_image: images/rails-configuration.jpg
canonical_url: false
description: Learn about my journey, learning how to set up a configuration file to streamline creating new Rails project.
---
So this article you can file away as what I learned today [#TIL](https://twitter.com/hashtag/TIL?src=hashtag_click). This surprised me so much I am writing to share. This is written specifically about [Ruby on Rails](https://rubyonrails.org/) and the initial set up.

## Details
When ever I start a new project with a CLI generate, I spend several minutes setting the environment up like I like it to begin developing, and I imagine any developer does the same.

For instance, when I start a React project using Create React App, it is always the same:
- Make `src/components` directory
- Copy over my dotfiles for the Eslint and prettier configuration I prefer.
- Copy my `renovatebot.json` configuration file
- Delete CSS Modules, install `sass`, and copy my Sass 7-in-1 boilerplate.

This was so annoying I created my own React Boilerplate to stream line this process. I will write about this very soon.

### Ruby on Rails
I started using [Ruby on Rails](https://rubyonrails.org) at [Flatiron School](https://flatironschool.com/) as a standalone and backend API. I have developed a methodology and found myself setting the environment up on this platform as well.

Today I read an article by Samuel Mullen [Configuring New Rails Projects](https://samuelmullen.com/articles/configuring_new_rails_projects_with_railsrc_and_templates/) and learned you can set up `.railsrc` and template files to streamline setup. I have researched some more and have developed a work in progress configuration for myself.

To get started make sure, in your user account route, create a dotfile: `.railsrc`.

### Database
When I first started using Rails, I used a [SQlite3](https://www.sqlite.org/index.html) database which Rails defaults. It was easy to setup and use, but after a few challenging deploys to [Heroku](https://www.heroku.com/), which supports [Postgres](https://www.postgresql.org/), I migrated to Postgres on all of my projects. So, the first switch I added to the `railsrc` file was the toggle for the Postgres DB (see below). Second I added a template file which I will detail below:

```bash
--database=postgresql
--template=/home/webrev/development/my-docs/rails-template.rb
```

## Template
Let us go over some of these settings numbered below:
1. I am installing a few gem and then run bundle install.
2. Set the generator to not install stylesheets when resources are generated because I will use a SCSS Boilerplate.
3. Create the Postgres DB's so I do not have to after I open the project.
4. Initialize a repo and make the Initial commit. This step is not perfect. It does as expected, but as the installation continues, not all of the changes are committed. I am still working on this step.

```ruby
#add guard-minitest and spring to dev - 1
gem_group :development do
  gem 'guard-minitest'
end

gem_group :test do
  gem 'minitest-rails-capybara'
end

run "bundle install"

#config generator defaults - 2
environment "config.generators do |g| \n g.stylesheets false \n end"

#create postgres DB for postgress.app - 3
run "psql -c 'CREATE DATABASE #{app_path}_development;'"
run "psql -c 'CREATE DATABASE #{app_path}_test;'"

#Initialize local Git repository and Initial Commit - 4
git :init
git add: "."
git commit: "-m 'Initial commit'"
```
To create a new project:
`rails new my-rails-project`

That is it... it creates my DB's, with no self typed command line switches. MAGIC. Well I can do some much more than just this but it is a start.
