---
title: Gridsome Post Comments
date: 2020-06-14
published: true
tags: ['gridsome', 'vuejs', 'webdev', 'tutorial']
series: false
cover_image: images/comments.jpg
canonical_rul: false
description: In this tutorial, we will look at how to add Disqus commenting into a Gridsome site. Since Gridsome is part of the Vue.js eco-system, these steps should work with any Vue.js site
---
In this tutorial, we will look at how to add Disqus commenting into a Gridsome site. Since Gridsome is part of the Vue.js eco-system, these steps should work with any Vue.js site.

## Setup Disqus
So, we are going to set up Disqus, an external service which uses an `iframe` to inject commenting on your site.

First step is to go to [Disqus](https://disqus.com/) and sign up for a free account. During the account setup choose the option: '**I want to install Disqus on my site**'. The next setting to watch for is when you are asked '**What platform is your site on?**', pick '**Universal Code**'. Make a note of the `Shortname` you assigned to distinguish your site, as we will use it later.

## Setup Plugin
To inject Disqus on your site, we will be using the Vue package `vue-disqus`. To add this package to your project: `yarn add vue-disque` or `npm install --save vue-disque`.

We are going to setup `vue-disqus` globally, but there are additional directions for getting up a locally in the [vue-disqus](https://ktquez.github.io/vue-disqus/setup.html#using) documentation.

Add to your `main.js`:
```javascript
import VueDisqus from 'vue-disqus'

export default function (Vue, { head })  {
  Vue.use(VueDisqus)
}
```
Next you need to add the component where you want the comments added. In my case, I am using Gridsome's default blog template, so I added the component to the Post template.

Insert the following:
```javascript
<Disqus shortname="mysiteshortname" :identifier="$page.post.title" />
```
**Note**: The above code does not agree with the current Gridsome documentation. I have prepared a [Pull Request](https://github.com/gridsome/gridsome.org/pull/444#issuecomment-641021540) to fix this error to correspond to the [vue-disqus](https://ktquez.github.io/vue-disqus/setup.html#globally) documentation.

Now everything just works. That is it.

## Optional Refactor
The above works, however if you are adding a comment block to many places in your site, we can abstract the `shortname` a little.

If you the include the `shortname` in the `main.js`, it will default throughout your site:

```javascript
import VueDisqus from 'vue-disqus'

export default function (Vue, { head })  {
     Vue.use(VueDisqus, {
      shortname: 'your-shortname-disqus'
    })
}
```
Then insert the component in your template like this:

```javascript
<Disqus :identifier="$page.post.title" />
```
