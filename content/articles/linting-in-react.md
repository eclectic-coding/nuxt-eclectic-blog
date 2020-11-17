---
title: Linting in React
date: 2020-01-26
published: true
tags: ['React','Productivity','Eslint']
series: true
cover_image: images/organized.jpg
description: Recently I started experimenting with the idea of code linting via ESLint and Prettier. This article covers the configuration of ESLint, Prettier, and Husky in a React project.
---
Recently I started experimenting with the idea of code linting via ESLint and Prettier. I recently wrote another article on this subject concerning this Nuxtjs Blog - [Webstorm and Nuxtjs Linting](https://eclecticsaddlebag.com/posts/webstorm-nuxtjs-linting). So, enter the story of three tools:

- **EsLint**: According to ESLint's website, "*ESLint is a tool for identifying and reporting on patterns found in ECMAScript/JavaScript code, with the goal of making code more consistent and avoiding bugs.*" This article explores how to setup ESLint and Prettier along with husky to format the code before a commit to GitHub.
- **Prettier**: Where as Prettier, is concerned with ensuring "*that all outputted code conforms to a consistent style.*"
- **Husky**: An NPM package which utilizes GIT [hooks](https://git-scm.com/docs/githooks) to integrate tools and increase quality code submissions to your repository. In addition, using these three together as a `pre-commit`hook strategy, means all contributors to your project submit with the same code standards.

**TLTR**: Go to the [repository](https://github.com/eclectic-coding/react-chucksmith-portfolio).


### Setup
To get started create a React App: `yarn create react-app react-linting` or `npx create-react-app react-linting`.  Create React App comes pre-configured with some of the necessary ESLint packages and we need to extend that default configuration. So, we need to install a few additional packages as `devDependencies`: `yarn add -D eslint-config-prettier eslint-plugin-prettier prettier` or `npm install --save-dev eslint-config-prettier eslint-plugin-prettier prettier`. These packages will set up the additional linting we require. Since, React uses the `package.json`for ESLint configuration, so will I, but you could just as easily is a separate configuration file. Add the prettier plugin to the `eslingConfig`section of the `package.json` file:
```json
"eslintConfig": {
    "extends": [
      "react-app",
      "plugin:prettier/recommended"
    ]
  },
```
Instead of a `.prettierrc` configuration file we will add a new section to the `package.json`file. This section is my configuration but you can change as you desire:

```json
"prettier": {
    "printWidth": 90,
    "bracketSpacing": true,
    "trailingComma": "es5",
    "semi": false,
    "singleQuote": true
  },
```

To make these tools more usable, we can add the following to the script section of our `package.json`file:

```json
"scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "lint:fix": "eslint src/**/*.js --fix",
    "lint": "eslint src/**/*.js",
    "prettify": "prettier src/**/*.js --write"
  },
```

We could stop here, but we are going to automate these task. You need to install two more packages: `yarn add -D husky lint-staged` or `npm install --save-dev husky lint-staged`. We need to add the following tweo short sections to the `package.json` file:

```json
"husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.js": "npm run lint:fix"
  },
```

So now the magic begins. Based on our prettier configuration, change something in a `javascript`file: add a semi-colon or change from single to double quotes. When you commit a change, `husky` will run `lint-staged`and fix your files automagically. This is just a very basic setup but you get the idea.

Happy coding.
