---
title: Rails Active Storage
date: 2020-09-13
published: true
tags: ['rails', 'ruby', 'webdev']
series: false
cover_image: images/active-storage.jpg
canonical_rul: false
description: On any web application, the ability to use images are tantamount. In  a Ruby on Rails project, using Active Storage increases the flexibility to use external storage services and to seamlessly create user interaction.
---

On any web application, the ability to use images are tantamount. In  a Ruby on Rails project, using Active Storage increases the flexibility to use external storage services and to seamlessly create user interaction.

In this article we will use Active Storage to allow a user to add an Avatar to their user profile. This Avatar will display on their Profile page and in the User Profile link in the Navbar.

TL;TR:  The completed repository if you would like to jump straight to the [code](https://github.com/eclectic-coding/article_active_storage).

## Setup

This is not a complete tutorial on the setup of our Rails project. Here are the basic features below. I do suggest you look at the repository.
- Basic Rails project with postgres
- A controller called `static` for the home page, and the root route specified as `static#home`.
- Add Bootstrap 4 using Webpacker
- Add Devise with the additional registration field of username added to the migration
- Use the gem `devise-bootstrapped` to preconfigure the Devise views with bootstrap styling.

Add the following Bootstrap navbar:
```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark mb-3">
  <div class="container">
    <%= link_to "Active Storage", root_path, class: "navbar-brand" %>

    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarNav">

      <ul class="navbar-nav ml-auto">

        <% if user_signed_in? %>
          <li class="nav-item">
            <%= link_to current_user.username, user_path(current_user.username), class: "nav-link" %>
          </li>
          <li class="nav-item">
            <%= link_to "Sign out", root_path, class: "nav-link" %>
          </li>
        <% else %>
          <li class="nav-item active">
            <%= link_to "Log in", root_path, class: "nav-link" %>
          </li>
          <li class="nav-item">
            <%= link_to "Sign up", root_path, class: "nav-link" %>
          </li>
        <% end %>
      </ul>
    </div>
  </div>
</nav>
```

Create a profile page for the user:
```html
<div class="d-flex align-items-center justify-content-center mt-5">
  <div class="media mr-5 align-self-start">
    Avatar
  </div>
  <div class="media">
    <div class="media-body">
      <div class="d-flex flex-row align-items-center justify-content-between">
        <h1><%= @user.username %></h1>
        <%= link_to "Edit", edit_user_registration_path, class: "ml-3 btn btn-secondary btn-sm" if current_user.id == @user.id %>

      </div>
    </div>
  </div>
</div>
```
Add the route for the profile page: `resources :users, only: [:show], param: :username, path: ""`

Again, this is an overview of the setup, just look at this [commit](https://github.com/eclectic-coding/article_active_storage/tree/dfb6af23e8234c65f0d818a7b66d04ddf0f7fd8e) for the complete [beginning source](https://github.com/eclectic-coding/article_active_storage/tree/dfb6af23e8234c65f0d818a7b66d04ddf0f7fd8e).

## Active Storage
Active Storage gives us the option to use different storage services. We start by configuring the development environment, by adding the following to `config/environments/development.rb`:
```ruby
# Store files locally.
config.active_storage.service = :local
```
If you want to use Amazon S3 service in production, you add the following to `config/environments/production.rb`:
```ruby
# Store files on Amazon S3.
config.active_storage.service = :amazon
```
Refer to the [documentation](https://edgeguides.rubyonrails.org/active_storage_overview.html#setup) for more configuration options in production.

Remember Active Storage is part of Rails, so you just need to install to configure: `rails active_storage:install`, then run the migration: `rails db:migrate`.

Active Storage uses two tables in your application’s database named `active_storage_blobs` and `active_storage_attachments`. The `active_storage_attachments` is a polymorphic join table that stores your model's class name.

In the `Gemfile`, uncomment and `bundle` install `image_processing`. This gem allows us to resize our images. Make sure to restart your rails server.

## Avatar

Add to the User model: `has_one_attached :avatar`:
```ruby
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable

  has_one_attached :avatar
end
```

We need to update the permitted parameter's method in the application controller for the `avatar`:

```ruby
class ApplicationController < ActionController::Base
  before_action :configure_permitted_parameters, if: :devise_controller?

  private

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:username])
    devise_parameter_sanitizer.permit(:account_update, keys: %i[avatar name username])
  end

end
```

## Edit Profile

First, let's update the 'Edit Profile' form, so we can select an Avatar. You will find the template in `app/views/devise/registrations/edit.html.erb`.

We need to add `name`, and `username` to the edit form:
```html
<div class="form-group">
    <%= f.label :username %>
    <%= f.text_field :username, autofocus: true %>
  </div>

  <div class="form-group">
    <%= f.text_field :name, autofocus: true, placeholder: "Name" %>
  </div>
```
We need to add a place to display the Avatar, and a form picker to select the file. This will be placed at the top of the file. Take a look at the complete [source](https://github.com/eclectic-coding/article_active_storage/blob/main/app/views/devise/registrations/edit.html.erb) for this file:

```html
<div class="row">
    <div class="col-sm-2">
      <% if current_user.avatar.nil? %>
        <%= image_tag f.object.avatar.variant(resize: "128x128!"), class: "rounded-circle m-4" %>
      <% end %>
    </div>
    <div class="col-sm-10">
      <div class="form-group">
        <%= f.label :avatar %>
        <%= f.file_field :avatar %>
      </div>
    </div>
  </div>
```
We are testing if there is an Avatar that is associated with the current user, then displaying the image by using an `image_tag`, resized to 128px, a feature of the `image_processing` gem. We are using the Bootstrap classes to display the avatar as a rounded circle.

## User Profile Page
So, we need to revisit our User `show.html.erb` template file to include the newly selected avatar.

```html
<div class="d-flex align-items-center justify-content-center mt-5">
  <div class="media mr-5 align-self-start">
    <% if current_user.avatar.nil? %>
        <%= image_tag f.object.avatar.variant(resize: "128x128!"), class: "rounded-circle m-4" %>
      <% end %>
  </div>
  <div class="media">
    <div class="media-body">
      <div class="d-flex flex-row align-items-center justify-content-between">
        <h1><%= @user.username %></h1>
        <%= link_to "Edit", edit_user_registration_path, class: "ml-3 btn btn-secondary btn-sm" if current_user.id == @user.id %>

      </div>
    </div>
  </div>
</div>
```
This is the same code we used for the edit template. This is the result:

!['Screenshot of User Profile with new Avatar'](../../static/images/user-profile.png)

## Navbar
Originally, I had placeholder paths in the Navbar, so we revisit to make the links workable. Also, we will include the avatar, and a check if one exist.
```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark mb-3">
  <div class="container">
    <%= link_to "Active Storage", root_path, class: "navbar-brand" %>

    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarNav">

      <ul class="navbar-nav ml-auto">

        <% if user_signed_in? %>
          <li class="nav-item">
            <%= link_to user_path(current_user.username), class: "nav-link" do %>
              <% if current_user.avatar.nil? %>
                <%= image_tag current_user.avatar.variant(resize: "24x24!"), class: "mr-1" %>

              <% end %>
              <%= current_user.username %>
            <% end %>
          </li>
          <li class="nav-item">
            <%= link_to "Sign out", destroy_user_session_path, method: :delete, class: "nav-link" %>
          </li>
        <% else %>
          <li class="nav-item active">
            <%= link_to "Log in", new_user_session_path, class: "nav-link" %>
          </li>
          <li class="nav-item">
            <%= link_to "Sign up", new_user_registration_path, class: "nav-link" %>
          </li>
        <% end %>
      </ul>
    </div>
  </div>
</nav>
```
## Not Perfect
So, this solution is not entirely practical. I do not like the approach where we are displaying a blank space if the avatar does not exist. In the next article we will build on this feature by using the users [Gravatar](https://en.gravatar.com/) as an optional or fallback image.


