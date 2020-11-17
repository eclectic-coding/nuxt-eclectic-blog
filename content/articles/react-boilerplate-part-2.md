---
title: React Boilerplate - Part 2
date: 2020-05-12
published: true
tags: ['react', 'javascript', 'beginners', 'tutorial']
series: Streamlined Workflow
cover_image: images/quality.jpg
canonical_rul: false
description: Welcome to week three in this article series, "Streamlined Workflow." In this week's article we will continue to build out a Boilerplate configuration for React. Last week we built out a working boilerplate but without much functionality. This week we will look at code quality, deployment enhancements, and styling.
---
Welcome to week three in this article series, "Streamlined Workflow." In this week's article we will continue to build out a Boilerplate configuration for React. Last week we built out a working boilerplate but without much functionality. This week we will look at code quality, deployment enhancements, and styling.

TLTR: If you want to go straight to the completed [code](https://github.com/eclectic-coding/medium-react-boilerplate)

## Subtle Changes to Parcel
Parcel generates the bundle on start up of the development server in the `dist` directory, unless you specify a custom directory. It also caches its operations in a hidden directory called `.cache`. Every so often, in my experience, the cache will get out of sync. So, let's create a `clean` script to remove the `.cache` and `dist` directories so they are compiled fresh. Also, we can create a `dev` script to combine the `clean` and `start` script. This will give the end user a little more flexibility.
```
"scripts": {
    "start": "parcel src/index.html --port 3000 --open",
    "dev": "npm run clean && npm run start",
    "build": "parcel build src/index.html",
    "clean": "rm -rf ./.cache ./dist"
  },
```

## Code Quality
So far we have developed a React Environment. From this point forward, we will build out some features to enhance the development experience, code quality, both in errors and formatting. The first tool we will install is [Eslint](https://eslint.org/), which "statically analyzes your code to quickly find problems," and automatically fix the errors. This program is highly customizable and includes many preset configuration for different environments. To get started, install the following packages:
```
yarn add -D eslint eslint-config-airbnb eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react 
```

Now we need to define our configuration. The default install of Create React App (CRA), uses a configuration object in the `package.json` like so:
```
"eslintConfig": {
    "extends": "react-app"
  },
```
Even though this is an acceptable approach, I prefer to use configuration dotfiles, because they are portable between projects and it limits the clutter to the `package.json`.

We need to create a configuration file for **Eslint**: `touch .eslintrc` in the project root and add the following to the file:
```
{
  "parser": "babel-eslint",
  "plugins": [
    "react"
  ]
}
```
**Code Format**  
Where Eslint finds potential syntactical errors in your code, [Prettier](https://prettier.io/) checks a specific set of rules to maintain an uniform code format. For instance:
- Single or double quotes?
- Semi-colons or not
- Spaces in brackets
- default document width

To set up Prettier, you need to install the following packages:
```
yarn add -D eslint-config-prettier eslint-plugin-prettier prettier
```
We need to create a configuration file for Prettier: `touch .prettierc` in the project root. Below you will find my preferences but these settings are up to the user or the project parameters:
```
{
  "printWidth": 90,
  "bracketSpacing": true,
  "trailingComma": "none",
  "semi": false,
  "singleQuote": true
}
```
A couple of the installed packages make sure that Eslint and Prettier integrate well together. We will need update the `.eslintrc`:
```
{
  "parser": "babel-eslint",
  "plugins": [
    "react"
  ],
  "extends": [
    "plugin:prettier/recommended"
  ],
  "rules": {
    "no-undef": "off"
  }
}
```
We need to add a few more scripts to our script section of the `package.json`:
scripts
```
"scripts": {
    "start": "parcel src/index.html --port 3000 --open",
    "dev": "npm run clean && npm run start",
    "build": "parcel build src/index.html",
    "clean": "rm -rf ./.cache ./dist"
    "lint:fix": "eslint src/**/*.js --fix",
    "lint": "eslint src/**/*.js",
    "prettify": "prettier src/**/*.js --write"
  },
 ```
Now you will be able:
- Check for linting errors: `yarn lint`
- Fix linting errors: `yarn lint:fix`
- Format code: `yarn prettify`

Lastly, make sure you configure your preferred Editor or IDE to use your Eslint/Prettier configuration.

## Pre-commit scripts
We have a solid Code Quality section built using Eslint and Prettier. The flaw in this system is that it doesn't necessarily ensure the code of those that contribute to your software project will be error free and formatted properly.

We are going to install and configure a tool called [Husky](https://github.com/typicode/husky). This package will lint and format all code using a Git pre-commit hook. This means that all commits will formatted and checked for errors before it is pushed to a remote repository.

Install the following packages:
```
yarn add -D husky lint-staged
```

Add the following to your `package.json`:
```
"husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.js": "npm run lint:fix"
  },
```

Now let's update our scripts:
```
"scripts": {
    "start": "parcel src/index.html --port 3000 --open",
    "dev": "npm run clean && npm run start",
    "build": "parcel build src/index.html",
    "clean": "rm -rf ./.cache ./dist"
    "lint:fix": "eslint src/**/*.js --fix",
    "lint": "eslint src/**/*.js",
    "prettify": "prettier src/**/*.js --write",
    "lint-test": "lint-staged"
  },
```

## New Features

**Environment Variables** - So almost every project I work on I have to store API keys or other sensitive data for the application's use. The good news about Parcel, is that there are no additional packages needed, like `dot-env`, it is already configured. You can read more on Parcel's [Environment Page](https://en.parceljs.org/env.html).

**ECMA Stage 2 Proposals** - The Transform Class properties plugin for Babel you will need to be effective using React. Again Parcel makes this easy. Install this package:
```
yarn add -D babel-plugin-transform-class-properties
```
We will need to update the `.babelrc` configuration:
```
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ],
  "plugins": [
    [
      "transform-class-properties"
    ]
  ]
}
```

## Styling
I am not going to add styling to this Boilerplate. Styling in React tends to be Opinionated. For instance, my preference is SASS, but yours may to CSS-in-JS or a framework like Bootstrap or Material UI. I encourage you to ask your self what you use the most? Or if you even want to add styling to this Boilerplate. Again, I have added my preference to my Boilerplate. This is a decision you will need to make based on your productivity preferences.

**Browserslist** - We are going to set up [Browserslist](https://github.com/browserslist/browserslist). This allows your styling engines to compile a style bundle based on a select set of browsers. These configurations will be added to the `package.json`. These are the default values used in CRA, which I will challenge:
```
"browserslist": {
    "production": [
      ">0.5%",
      "not dead",
      "not op_mini all"
    ],
```
Browserslist default setting recommends: `> 0.5%, last 2 versions, Firefox ESR, not dead`

I would encourage you to study [StatCounter](https://gs.statcounter.com/) and your own site traffic to determine what works for you and set the your defaults in your boilerplate.

This is a good solid Boilerplate. Try it out and leave comments or suggestions.

Next week we will look at another use for Parcel.




