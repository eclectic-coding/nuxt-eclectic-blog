---
title: Rails Code Along
date: 2020-08-30
published: true
tags: ['rails', 'testing', 'ruby']
series: false
cover_image: images/rails.jpg
canonical_rul: false
description: So I am always looking for ways to increase my skills on the Ruby on Rails platform. As I look for first professional dev job, afters years as a hobbyist, I have to say, Rails would be by dream job.
---

So I am always looking for ways to increase my skills on the Ruby on Rails platform. As I look for first professional dev job, afters years as a hobbyist, I have to say, Rails would be by dream job. I have been working more recently with RSpec TDD/BDD development and learning proper methodology.

TLTR: Hey, just go grab the [code][REPO]

## Basic Course Description
I have started recently a Udemy course by Jordan Hudgens, called [Professional Rails Code Along][COURSE]. The structure of the course is to develop a client enterprise application, using TDD. The course is three years old and a little dated as it uses Rails 5 and Bootstrap 3, but is still a very solid course.

I decided to update the application along the way. So, the first upgrade was to use the latest Rails release (v. 6.0.3.2) and utilize Webpacker as much as feasibly possible. So the first two enhancements I made:
- Rails 6 with Postgres
- Bootstrap 4.5.2 using Webpacker. Honestly, it is too easy not is use the [Railsbytes Template][BOOTSTRAP]. With one command you are setup:
```
rails app:template LOCATION='https://railsbytes.com/script/x9Qsqx'`
```

## Enhance Development Environment
Also as I proceeded I wanted a more robust developers experience.

Additional enhancements to development environment
- Guard for automated testing
- Fuubar for a reporter progress bar
- Simplecov for code coverage reports
- Rubocop for code consistency

Read more about this setup in a previous article about [Rails Testing Setup](/rails-test-setup)

## Gritter Notifications

The default tutorial uses the [Gritter]() gem to implement Growl pop-up notifications in the application, which uses a jQuery plugin called `gritter`. So, I wanted to utilize Webpacker, luckily [Mbonu Blessing](https://dev.to/nkemjiks) has written a wonderful tutorial on [Using Gritter in Rails 6][GRITTER]. The only change I made to this tutorial was with regards to styling. I had difficulty using the default styling in the Yarn package, and I did not want to reinvent the CSS and images. The simple fix for my was to use a CDN of the gritter styling. I added the following to `app/views/layouts/application.html.erb` in the `head`section:

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/jquery.gritter/1.7.4/css/jquery.gritter.css">
```

It worked perfectly. 

I know this is a shorter article but I am just starting. Pay attention as I document the journey through this tutorial and the enhancements along the way. 




[REPO]: https://github.com/eclectic-coding/overtime_app
[COURSE]: https://www.udemy.com/course/professional-ruby-on-rails-coding-course/
[BOOTSTRAP]: https://railsbytes.com/public/templates/x9Qsqx
[GRITTER]: https://dev.to/nkemjiks/using-gritter-in-rails-6-52fe
