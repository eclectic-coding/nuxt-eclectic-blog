---
title: React Boilerplate - Part 1
date: 2020-05-06
published: true
tags: ['react', 'javascript', 'beginners', 'tutorial']
series: Streamline Workflow
cover_image: images/forklift-packages.jpg
canonical_rul: false
description: Welcome to week two in this article series, "Streamline Workflow." In this week's article we will explore a Boilerplate configuration for React. The idea to to have a turnkey solution, so you can start developing quickly, instead of wasting time reconfiguring the default Create React App generated starter.
---
Welcome to week two in this article series, "Streamline Workflow." In this week's article we will explore a Boilerplate configuration for React. The idea to to have a turnkey solution, so you can start developing quickly, instead of wasting time reconfiguring the default Create React App (CRA) generated starter, based on your preferred developing environment.

## Options
So there are a few approaches we can take:

**Stick with Create React App** - You can take this approach and reconfigure for your preferences each time. The other variation of this approach is to maintain a modified copy of CRA locally setup the way you like. This could work but remember you will have to maintain and keep the packages updated.

**Create a Custom Boilerplate with Webpack** - You can roll you own. I have done this in the past. It exposes the Webpack configuration more than the existing CRA does and allows you to fully customize the environment. If you are interested in this approach [Robin Wieruch](https://www.robinwieruch.de/minimal-react-webpack-babel-setup) wrote a wonderful tutorial series on how to accomplish this boilerplate. This was my original approach and you can look at and use the code on my [Repository](https://github.com/eclectic-coding/react-boilerplate) if this is a direction you wish to try. In the end I decided, this was to complex for the average beginning end user to maintain.

**Create a Custom Boilerplate w/Webpack** - This will be the topic of this two part series on a React Boilerplate.

## Why Parcel
Well, according to Parcel's website, it is a "blazing fast, zero configuration web bundler." This is mostly accurate and we are going to look at the simplicity of this design. Where the Webpack bundler was complex for the beginner, this is super easy, *and* still robust.

TLTR: If you want to go straight to the [code](https://github.com/eclectic-coding/medium-react-boilerplate)

## Set up
I am using yarn for this tutorial, but you can use any package manager you are familiar.
First let's get the project setup:
- Create a new project directory: `mkdir my-react-boilerplate`
- Browse to the directory: `cd my-react-boilerplate`
- Create a package.json: `yarn init -y`
- Create a src directory `mkdir src`
- Create `index.js` in the `src` directory: `touch src/index.js`

Your initial `package.json` should look similar to this:
```
{
  "name": "my-react-boilerplate",
  "version": "1.0.0",
  "main": index.js",
  "license": "MIT"
}
```
You need to change the `main` to `src/index.js`

## Adding Parcel
To start, let's setup React, Babel, and our package bundler:
```
yarn add react react-dom
yarn add -D parcel-bundler @babel/core @babel/preset-env @babel/preset-react
```
In the project root, create a configuration file for Babel called `.babelrc` with the following content:
```
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}
```

Add a few scripts to start and build the project:
```
"scripts": {
  "start": "parcel src/index.html"
  "build": "parcel build src/index.html"
}
```
I use the `src` directory for most of my content, so, create `index.html` in the `src` directory: `touch src/index.html`. In the `index.html` file place the following content:
```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>React Project</title>
</head>
<body>

</body>
</html>
```
So far your project structure should look like this:
```bash
my-project
├── .babelrc
├── .gitignore
├── LICENSE
├── package.json
├── README
├── renovate.json
├── src
│   ├── App.js
│   ├── index.html
│   └── index.js
└── yarn.lock
```
## Setup React

Create index.js: `touch src/index.js`

Add a `<div>` tag to the body of the `index.html with id=app, and add the `index.js` file as such:
```html
<body>
    <div id="app"></div>
    <script src="index.js"></script>
</body>
```
Parcel will use the `id` in the root `div` and the `script` tag to automatically build a template in the bundle in the created `dist` folder. Let's add to following to the `index.js` file:
```js
import React from 'react';
import { render } from 'react-dom';

const App = () => <div>Hello World!</div>;

render(<App />, document.getElementById('app'));
```
This is a minimalist approach and will technically work. However, if we are building a boilerplate to streamline our workflow, it is honestly not very practical. Let's refactor our setup.

## Refactoring React

Create an `App.js` in the `src` folder and add the following content:
```js
import React from 'react'

const App = () => {
  return (
    <div>
      <h1>Hello from App.js</h1>
    </div>
  )
}

export default App

```

Refactor the `index.js` file:
```
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'

ReactDOM.render(<App />, document.getElementById('app'))

```
Okay that is it, you are ready to start the development server: `yarn start`. You can open your browser at `http://localhost:1234`

## Refactor Parcel
So, in my opinion there are a few changes we can make:
- First, I do not like the starting port
- Also, it would be nice if the browser would automatically open after the development server starts.

You can edit the start up script very easily to accommodate these changes: `"start": "parcel src/index.html --port 3000 --open"`.

This is a very simple setup, with a package bundler with almost zero configuration. However, the boilerplate has a lot of areas we can fill in.

Next week:
- More tweaks to Parcel startup scripts
- Setup browserlist
- Configure `eslint` and `prettier`
- Configure scripts for linting
- Configure `husky` to lint the source code as a `pre-commit` hook
- Setup project styling

Stay tuned.




