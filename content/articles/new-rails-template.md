---
title: New Rails Template
date: 2020-09-27
published: true
tags: ['rails', 'ruby', 'webdev']
series: Streamlined Workflow
cover_image: images/template.jpg
canonical_rul: false
description: In this article we take a look at a more complete Rails template, to start a new Rails project. Why? With a custom template, you can start developing your project immediately already preconfigured with the gems, packages, and settings you like to use.
---

In this article we take a look at a more complete Rails template, to start a new Rails project. Why? With a custom template, you can start developing your project immediately already preconfigured with the gems, packages, and settings you like to use.

## Credit
The inspiration for my custom template starts with Andy Leverenz's [Kickoff template](https://github.com/justalever/kickoff_tailwind). There are a few other great resources as well.

**Jumpstart** Chris Oliver has put together a template called [Jumpstart](https://github.com/excid3/jumpstart). This template creates a Boilerplate app that includes user accounts, admin interface, and Bootstrap styling.

**Jumpstart Pro** Chris also has a commercial product called [Jumpstart Pro](https://jumpstartrails.com/) that bootstraps a full featured e-commerce application setup.

## Template Features

- User accounts with Devise, friendly_id, and name_of_Person
- Sidekiq
- Testing environment with RSpec, Guard, Simplecov, and Rubocop
- User Avatars using Active Storage, with a fallback image using Gravatar
- Tailwind for styling
- Fontawesome
- Stimulus JS and `tailwindcss-stimulus-components` for the dropdown menu action

TL;TR:  The completed repository to the finished custom rails template if you would like to jump straight to the [code](https://github.com/eclectic-coding/eclectic_template).

## Using a Template
First let's cover the basics of using a template. The `rails new` command offers many command option flags, and provides a flag to use a custom template:
```
rails new myapp -m /path/to/my/template.rb

```
I had written on this topic previously in [Rails Boilerplate](/rails-boilerplate)<!-- @IGNORE PREVIOUS: link -->. When this command is executed, Rails will run the script, install and create the new app.

Now, to streamline this process you can create a dotfile in the root of your file system: `.railsrc`. Rails will look for this configuration file first. In our case this is a good start:
```
--database=postgresql
-T
-m /path/to/my/template.rb
```
So, we can start the new project: `rails new myapp`. Rails will use Postgres database, will not install a testing framework (more on this later), and use our template, without having to type the commands on the command line.

If at some time you wan to create a default Rails app, instead of your custom setup: `rails new myapp --no-rc`.

## A Custom Template
If you start to look at the main `template.rb` in the [codebase](https://github.com/eclectic-coding/eclectic_template/blob/main/template.rb) you will notice at the top of the file is a series of methods, and the business logic of calling these methods:
```ruby
# Main setup
source_paths

add_gems

after_bundle do
  stop_spring
  add_testing
  add_active_storage
  add_users
  remove_app_css
  add_sidekiq
  add_foreman
  copy_templates
  add_tailwind
  add_fontawesome
  add_stimulus
  add_friendly_id
  copy_postcss_config
  add_stimulus_navbar

  # Migrate
  rails_command "db:create"
  rails_command "db:migrate"

  git :init
  git add: "."
  git commit: %Q{ -m "Initial commit" }
```
Basically, after the `bundle install` in completed the methods are executed, it creates the database, a migration is run, and then the first `git commit` is saved.

So, let's go over a few of the methods to show you the power of the API.

## Add gems
The `add_gem` method does not install these gems, it only adds the gems to the Gemfile. If you examine the method you will notice we are setting up gems associated with users, setting up a testing environment, and Rubocop.
```ruby
def add_gems
  gem "devise", "~> 4.7", ">= 4.7.2"
  gem "friendly_id", "~> 5.3"
  gem "image_processing"
  gem "sidekiq", "~> 6.1", ">= 6.1.1"
  gem "name_of_person", "~> 1.1", ">= 1.1.1"

  gem_group :development, :test do
    gem "database_cleaner"
    gem "factory_bot_rails", git: "http://github.com/thoughtbot/factory_bot_rails"
    gem "rspec-rails"
  end

  gem_group :development do
    gem "fuubar"
    gem "guard"
    gem "guard-rspec"
    gem "rubocop"
    gem "rubocop-rails", require: false
    gem "rubocop-rspec"
  end

  gem_group :test do
    gem 'simplecov', require: false
  end
end
```
NOTE: After your app is set up you will probably want to clean up your Gemfile. All the above gems and groups are added to the bottom of the Gemfile.

If you have been following my articles you know I have written about this [Rails Testing Setup](/rails-testing-setup)<!-- @IGNORE PREVIOUS: link --> before.

## Testing
I noted previously, in the `.railsrc` file, we did not install the default Rails testing environment. In the `add_testing` method we are setting up Rspec.
```ruby
def add_testing
  generate "rspec:install"

  directory "spec", force: true

  run "rm -r test" if Dir.exist?("test")

  copy_file "config/webpacker.yml", force: true
  copy_file ".rspec", force: true
  copy_file ".rubocop.yml"
  copy_file ".simplecov"
  copy_file "Guardfile"
end
```
Remember, this method will run after `bundle install`, so we are only configuring the gems here. Since we are using a repository, instead of just a simple template file, we can copy preconfigured files and directories:
- We are copying the `spec` directory and force overwriting the directory in the new app.
- In case the user does not have -T in the `.railsrc` file, we are deleting the `test` directory if it exists.

You can read more about the Template API at [Railsguides](https://guides.rubyonrails.org/rails_application_templates.html).

## Modifying Files
In the creating process we can also modify files. For instance, in the `add_tailwind` method:
```ruby
append_to_file("app/javascript/packs/application.js", 'import "stylesheets/application"' + "\n")
  inject_into_file("./postcss.config.js",
                   "let tailwindcss = require('tailwindcss');\n", before: "module.exports")
  inject_into_file("./postcss.config.js", "\n    tailwindcss('./app/javascript/stylesheets/tailwind.config.js'),", after: "plugins: [")

```
In this case we can `append_to_file` an import. Also, we are injecting into a file at a specific point (before of after some text).

We can also inject at the end of a file a block of inserts:
```ruby
inject_into_file 'app/javascript/controllers/index.js' do
<<~EOF
  // Import and register all TailwindCSS Components
  import { Dropdown, Modal, Tabs, Popover, Toggle } from "tailwindcss-stimulus-components"
  application.register('dropdown', Dropdown)
  application.register('modal', Modal)
  application.register('tabs', Tabs)
  application.register('popover', Popover)
  application.register('toggle', Toggle)
EOF
end
```

The Template API is fairly robust and offers a way to streamline the startup time of a new project. This is my personal setup. Clone the repository and give it a try, tweak it for your own, and if something doesn't work, open a PR.


