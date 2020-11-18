---
title: Conditional CSS Classes in HAML
date: 2020-08-09
published: true
tags: ['rails', 'css', 'webdev']
series: false
cover_image: images/letters.jpg
canonical_rul: false
description: I have been working on a new Rails side project called Your Congress, and decided to use HAML, instead of the standard Rails templating language (ERB). Why? Good question. Actually, I just wanted to learn how to use it, to include a new technology in my skills for the Job Search.
---
I have been working on a new Rails side project called [Your Congress](https://yourcongress.co), and decided to use HAML, instead of the standard Rails templating language (ERB). Why? Good question. Actually, I just wanted to learn how to use it, to include a new technology in my skills for the Job Search.

## What is HAML
According to the [HAML](https://haml.info/) web site:
> Haml (HTML abstraction markup language) is based on one primary principle: markup should be beautiful. It’s not just beauty for beauty’s sake either; Haml accelerates and simplifies template creation down to veritable haiku.

For those not familiar with templating in Rails, HAML is very similar to  the Pug templating language. The difference is that it allows injections of embedded Ruby commands.

A few rules:

HTML tags are preceded by `%` and include no closing tag:

```%button "Click here"```

or with classes attached and separated by a dot. Also notice the second line is indented. This is important with HAML, Pug, and other languages like Python.
```
%button.btn.btn-primary
  "Click here"
  ```

## Problem
My problem was that I needed to apply a custom class based on the condition of if the user was logged in or not. In ERB this may look like so, using Tailwind classes:

```html
  <button class="<%= 'cursor-not-allowed' if logged_in? %>">
```
My beginning HAML string was the following:
```
%button{type: "button", class: "text-white py-3 px-4 rounded-lg bg-indigo-700"}
  "Button text"
```
You can string different elements together within `{ }`.

## HAML solution
Come to find out, HAML has a special syntax for embedding logic into a tag string.
- First you need to enclose your classes with in square brackets `[]`.
- Inside the brackets, you enclose the default classed within `( )` followed by a `,`.
- The logic expression is enclosed in a second pair of `( )`.

```
class: [("text-white py-3 px-4 rounded-lg bg-indigo-700"), ("rounded opacity-50 cursor-not-allowed" unless logged_in?) ]}
```
Notice in the second expression, that if the user is not logged_in? we are using the additional classes.

So, the complete logic is:
```haml
%button{type: "button", class: [("text-white py-3 px-4 rounded-lg bg-indigo-700"), ("rounded opacity-50 cursor-not-allowed" unless logged_in?) ]}
  %p.work-sans.font-semibold.text-sm.tracking-wide
    - if !logged_in?
      'Log in to save to a follow list'
    - else
      =link_to 'Add to Follow List', edit_user_follow_list_path(mem_id: @senator[:member_id], user_id: current_user[:id])
```
After I figured this out, I can tell you I am liking HAML even more. I am finding it to be a concise elegant way of developing views in Rails.


