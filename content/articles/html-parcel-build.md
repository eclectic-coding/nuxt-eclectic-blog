---
title: HTML Parcel Build
date: 2020-05-31
published: true
tags: ['html', 'javascript', 'beginners', 'tutorials']
series: Streamlined Workflow
cover_image: images/tile-shapes.jpg
canonical_rul: false
description: So far in this series, we have looked at the benefits of using a Boilerplate to streamline your workflow, and a two articles on how to setup a React Boilerplate with [parcel][parcel] instead of Webpack. In this article we will explore a proposed workflow instead of traditional means to compile and bundle a traditional HTML/SCSS project using Parcel.
---

So far in this series, we have looked at the benefits of using a Boilerplate to streamline your workflow, and a two articles on how to setup a React Boilerplate with [parcel][parcel] instead of Webpack. In this article we will explore a proposed workflow instead of traditional means to compile and bundle a traditional HTML/SCSS project using Parcel.

TLTR: Want to just view the source code? Check out the article [repository][code].

## Setup

So, lets look at a beginning project:

1. Create a project directory: `mkdir html-project` and `cd` into the directory.
2. Create an starting page: `touch index.html`.
3. I like to add JavaScript and Styles in a `src` directory. So create them like so: `mkdir -p src/js src/styles`

Include the following into the `index.html` file:
```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport"
        content="width=device-width, initial-scale=1.0, minimum-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
<h1>Page Title</h1>
<p>Be <b>bold</b> in stating your key points. Put them in a list: </p>
<ul>
  <li>The first item in your list</li>
  <li>The second item; <i>italicize</i> key words</li>
</ul>
<p>Improve your image by including an image. </p>
<p><img src="https://placekitten.com/200/300" alt="A Great HTML Resource"></p>
<p>Add a link to your favorite <a href="https://www.dummies.com/">Web site</a>.
   Break up your page with a horizontal rule or two. </p>
<hr>
<p>Finally, link to <a href="https://chucksmith.dev">my Portfolio Site</a>. </p>
<!-- And add a copyright notice.-->
<p>&#169; Eclectic Coding, 2020</p>

</body>
<script>

</script>
</html>

```

## Styling

This is the opinionated section. I prefer to use a 7-in-1 SCSS structure which creates a more modular experience, and is easier to maintain. You can access my [Sass Boilerplate][sass] and import into the project.

The browser does not understand SCSS files so they need to be compiled and there are a few different ways to accomplish this. First we need to add a package to our project. There are two different packages you can use: [sass][dart-sass], Dart Sass, the primary implementation of Sass, which means it gets new features before any other implementation. Or [node-sass][node], which is the package we will use.

First, we need to generate a `package.json` file. I will use `yarn` but you can use your preferred package manager: `yarn init -y`. The package should look similar to this (note: I have expanded the author object):
```json
{
  "name": "article_html-parcel",
  "version": "1.0.0",
  "main": "index.js",
  "author": {
    "name": "Chuck Smith",
    "email": "chuck@eclecticsaddlebag.com"
  },
  "license": "MIT"
}
```
Now install node-sass as a development package: `yarn add -D node-sass`.

The SCSS structure uses SCSS partials which are called in `src/styles/main.scss` file. So, to compile into a main stylesheet. We issue the following command from inside the project directory:

```node-sass stylesheets/main.scss dist/main.css```

We need to add a link to the new stylesheet in the head of our `index.html` file:
```
<link rel="stylesheet" href="main.css">
```
So when you reload the page you will see an immediate change as styles are applied. In this case, a different font, a padding of 3rem all around, and font-size of 20px.

The problem is when we make changes to the SCSS partials and we have to recompile. The node-sass package does have a watch switch, but I have often found it not very robust:
```node
node-sass -w stylesheets/main.scss dist/main.css
```
The watch command does not watch and compile JavaScript. There are a few solutions: Grunt and Gulp for instance. I used Gulp for years and relied a long on the work Chris Ferdinandi did on a [gulp-boilerplate][gulp]. I would modify it for my needs on different projects and it works extremely well.

## Parcel
What about Parcel? We were introduced to it in the React Boilerplate. If we want to compile SCSS, Javascript, and bundle a project it is quite easy.

We need to install Parcel: `yarn add -D parcel@next`.

We need to add a few scripts to our `package.json`:
```json
{
  "name": "article_html-parcel",
  "version": "1.0.0",
  "main": "index.js",
  "repository": "https://github.com/eclectic-coding/article_html-parcel",
  "author": {
    "name": "Chuck Smith",
    "email": "chuck@eclecticsaddlebag.com"
  },
  "license": "MIT",
  "scripts": {
    "start": "parcel index.html --port 3000 --open",
    "dev": "npm run clean && npm run start",
    "build": "parcel build src/index.html",
    "clean": "rm -rf ./.cache ./dist"
  },
  "devDependencies": {
    "node-sass": "^4.14.1",
    "parcel": "^1.12.4"
  }
}

```
When we run `yarn start` script, parcel creates a `dist/` directory, compiles the `scss`, and JavaScript we have indicated in the `index.html`, and creates bundles in the new `dist directory.

Update the SCSS stylesheet link. In the earlier implementation, we pointed to a compiled stylesheet, but by pointing to a :
```html
  <link rel="stylesheet" href="src/styles/main.scss">
```
Parcel compiles everything and places the new bundle in the `dist` directory including hot reloading any changes to the html of styling.

If you have a main JS file, add a `script` tag in your HTML document pointing to your JS file. Parcel will bundle that as well.

BOOM! That's it.

[code]: https://github.com/eclectic-coding/article_html-parcel
[parcel]: https://parceljs.org/
[sass]: https://github.com/eclectic-coding/my_sass-boilerplate
[dart-sass]: https://github.com/sass/dart-sass
[node]: https://github.com/sass/node-sass
[gulp]: https://github.com/cferdinandi/gulp-boilerplate
