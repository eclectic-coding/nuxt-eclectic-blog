---
title: Rails Search with pg_search
date: 2020-10-18
published: true
tags: ['rails', 'postgres','tutorials']
series: false
cover_image: images/rails-search.jpg
canonical_rul: false
description: A search bar is an especial feature as a web application grows. There are several ways to implement this feature in a Rails application. This article will explore one of these, by searching a Postgres database with the `pg_search` gem.
---
A search bar is an especial feature as a web application grows. There are several ways to implement this feature in a Rails application. This article will explore one of these, by searching a Postgres database with the `pg_search` gem.

## Install pg_search

In a smaller application you can  query the database using ActiveRecord, a simple way to prototype search and filtering. It allows you to quickly find related records for databases of fewer than 1000 records, but, as your database grows, ActiveRecord queries can get overly complex and make your app lag.

Install the gem as usual. Add `gem pg_search` to your `Gemfile` and `bundle install`.

There are two basic search configurations with `pg_search`, a Single Model search scope or a multi Model configuration. In my case I am only using the Single Model configuration, but you can read more about [multi-search](https://github.com/Casecommons/pg_search#multisearchable) in the documentation.

I am using my [Rails Your Congress](https://github.com/eclectic-coding/rails_your_congress) as an example, and I am setting up a search bar on the Senator's search page.

## Model
To start using `pg_search`, you need to include `PgSearch` in your model and set up the `pg_search_scope`:
```ruby
class Senator < ApplicationRecord
  include PgSearch

  scope :sorted, ->{ order(last_name: :asc) }

  pg_search_scope :global_search,
                  against: [:first_name, :last_name, :state],
                  using: { tsearch: { prefix: true } }
end
```
In this example, I am searching from the Senator resource, by `first_name`, `last_name`, and `state`.

## Controller
Since, I am providing a search on the index page, the index method in the Senators controller:
```ruby
class SenatorsController < ApplicationController

  def index
    if params[:query].present?
      @senator_search = Senator.global_search(params[:query])
      @senators = @senator_search.paginate(page: params[:page], per_page: 20)
    else
      @senators = Senator.paginate(page: params[:page], per_page: 20)
    end

    respond_to do |format|
      format.html
      format.json { render json: { senators: @senators } }
    end
  end
  ```
  In this method, we set a conditional to determine if we are searching or not. If we are searching we are setting two instance variables. The variable `@senators` is used in the views, and I had to set up two variables to get `will_paginate` to play nicely with the `pg_search` results.

## Improvements
With each search the entire page re-renders. So next week we will look at how I set up StimulusJS to make the Senator container reactive.


