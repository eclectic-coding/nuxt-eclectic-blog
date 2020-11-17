---
title: Rails DRY Controllers
date: 2020-09-06
published: true
tags: ['rails', 'ruby', 'webdev']
series: false
cover_image: images/rails-dry.jpg
canonical_rul: false
description: In any programming language, it is important to create DRY (Don't Repeat Yourself) code. In this article we will explore three methods of writing Rails CRUD controllers, and use a gem to abstract controllers even more.
---

In any programming language, it is important to create DRY (Don't Repeat Yourself) code. In this article we will explore three methods of writing Rails CRUD controllers, and use a gem to abstract controllers even more.

TL;TR:  The completed repository if you would like to jump straight to the [code](https://github.com/eclectic-coding/article_rails_dry).

## Setup

Create a new rails project: `rails new rails_dry --database=postgresql` and run the migrations: `rails db:migrate`.

We will create three resources to look at standard CRUD operations :

- Article
- Blog
- Post

All three resources work the same and are centered around articles for a Blog type site. There are no user accounts, so this is not an example of a robust application. Instead it is to show three ways to write CRUD controllers. Therefore, we are looking at three resources which are basically identical to each other, just coded a little differently.

Each resource controller is set up with a `before_filter` for the resource, and `strong_params`. For instance with the `Articles` controller:
```ruby
class ArticlesController < ApplicationController
  before_action :set_article, only: %i[show edit update destroy]
  ...
    
  private

  # Use callbacks to share common setup or constraints between actions.
  def set_article
    @article = Article.find(params[:id])
  end

  # Only allow a list of trusted parameters through.
  def article_params
    params.require(:article).permit(:title, :content)
  end
end
```

Also, in each resources, I have added validation to the model (Article example):

```ruby
class Article < ApplicationRecord
  validates :title, :content, presence: true

end
```

## Article

So, I setup a basic controller. This controller represents how I was taught in Bootcamp, and represents a simple way to code a controller.  We will review the create method:

```ruby
def new
    @article = Article.new
end

def create
  @article = Article.create(article_params)
  if @article.save
    flash[:notice] = "Article was successfully created."
    redirect_to articles_path
  else
    render :new
  end
end
```

In this example, if the article is successfully created, you are redirected to the articles index page. If there is an error, the user is directed to the create page. You can view the [Articles Controller](https://github.com/eclectic-coding/article_rails_dry/blob/main/app/controllers/articles_controller.rb)

## Blog

For the Blog resource we will use the rails scaffold generator:

```ruby
rails g scaffold Blog title content:text
```

Remember to run your migrations: `rails db:migrate`.

```ruby
# GET /blogs/new
def new
  @blog = Blog.new
end

# POST /blogs
# POST /blogs.json
def create
  @blog = Blog.new(blog_params)

  respond_to do |format|
    if @blog.save
      format.html { redirect_to @blog, notice: 'Blog was successfully created.' }
      format.json { render :show, status: :created, location: @blog }
    else
      format.html { render :new }
      format.json { render json: @blog.errors, status: :unprocessable_entity }
    end
  end
end
```

The scaffold generates code using meta programming.  The `respond_to` block  helper method is attached to the Controller class (or rather,  its super class).  It is referencing the response that will be sent to  the View (which is going to the browser). The block shown above is formatting data - by passing in a  'format' parameter in the block - to be sent from the controller to the  view whenever a browser makes a request for html or json data.

Notice the similarities:

- In one line of code, after the successful save, it redirects and sends a message.
- The redirect on the error is basically the same as the Article resource.

**Problems** - Well, not *really* a problem, but is gets verbose in a hurry. However,  the `respond_to` block is somewhat easier to read than the `Articles`.

You can view the [Blogs Controller](https://github.com/eclectic-coding/article_rails_dry/blob/main/app/controllers/blogs_controller.rb)

## Post

This resource we are going to take a different approach. As we noted in the Blog resource, the controllers are cleaner, but are quite verbose. We are going to install the [`responders`](https://github.com/plataformatec/responders) gem. This gem adds support for `respond_with` which was removed in Rails 4.2, and allows us to do a lot of cool stuff. Let's setup the `responders` gem:

- Add to your Gemfile: `gem "responder"` and `bundle install`
- Install responders: `rails g responsers:install`
- Remember to restart your `rails server`

The gem installation adds the following configuration to the `application_controller`:
```ruby
require "application_responder"

class ApplicationController < ActionController::Base
  self.responder = ApplicationResponder
  respond_to :html

end
```

Now generate the `Post` resource using rails scaffold:

```ruby
rails g scaffold Post title content:text
```

Remember to run your migrations: `rails db:migrate`.

Notice how much cleaner the controller methods are? We can define the `respond_to` format for the controller under the `before_filter`.  In this case, the generator specified `:html` because it is fined in the `application_controller`.

```ruby
class PostsController < ApplicationController
  before_action :set_post, only: %i[show edit update destroy]

  respond_to :html

  ...

  def new
    @post = Post.new
    respond_with(@post)
  end


  def create
    @post = Post.new(post_params)
    @post.save
    respond_with(@post)
  end
  
  ...
    
end

```

What if you want the `:json` format in only the index view? It is so simple!

```ruby
class PostsController < ApplicationController
  before_action :set_post, only: %i[show edit update destroy]

  respond_to :html
  respond_to :json, only: :index
```

The `create` method is three lines of code. After the post is successfully saved it redirects, or throws errors if the validations are not successful. This creates a really DRY controller with a lot of flexibility.

You can view the [Posts Controller](https://github.com/eclectic-coding/article_rails_dry/blob/main/app/controllers/posts_controller.rb)

## Footnote

This has been fun. Leave a comment or send me a DM on [Twitter](http://twitter.com/EclecticCoding).

Shameless Plug: If you work at a great company and you are in the market for a Software Developer with a varied skill set and life experiences, send me a message on [Twitter](http://twitter.com/EclecticCoding) and check out my [LinkedIn](http://www.linkedin.com/in/dev-chuck-smith).
