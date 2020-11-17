---
title: 'Webstorm and Nuxtjs Linting'
date: 2020-01-07
published: true
tags: ['Webstorm','Nuxtjs','Eslint']
series: false
cover_image: images/webstorm-lint.jpg
canonical_url: false
description: Recently I starting experimenting with Vue.js and Nuxt.js, partly out of curiosity and partly to learn a new framework, before I start a job search in a few months. This article is about my troubleshooting getting Nuxt.js, Linting, and Webstorm to play nicely together.
---
Recently I starting experimenting with [Vue.js](https://vuejs.org/) and [Nuxt.js](https://nuxtjs.org/), partly out of curiosity and partly to learn a new framework, before I start a job search in a few months. This article is about my troubleshooting getting Nuxt.js, linting, and [Webstorm](https://www.jetbrains.com/webstorm/) to play nicely together. Webstorm in its default configuration, uses code formating based on [Standard JS](https://standardjs.com/) but can easily be configured to new standards.

**TLTR**: Go to the [repository](https://github.com/eclectic-coding/nuxt-linting).

## Introduction
The first time I created a Nuxtjs project and opened [Webstorm]() and used the [Reformat Code](https://www.jetbrains.com/help/webstorm/reformat-file-dialog.html) tool, my world turned red (36 errors). Again, the purpose of this article is to troubleshoot getting Nuxt.js, Linting, and Webstorm to play nicely together.

![eslint-error.png](../../static/images/eslint-error.png)

### Setup
To get started create a Nuxt App: `yarn create nuxt-app nuxt-linting` or `npx create-nuxt-app nuxt-linting`. I am selecting the defaults but I add Axios (although not required for thus tutorial), select both Eslint, and Prettier. You output should look something like below:

```bash
Create-nuxt-app v2.12.0
✨  Generating Nuxt.js project in nuxt-linting
? Project name nuxt-linting
? Project description My Nuxt.js project
? Author name eclectic-coding
? Choose the package manager Yarn
? Choose UI framework None
? Choose custom server framework None (Recommended)
? Choose Nuxt.js modules Axios
? Choose linting tools ESLint, Prettier
? Choose test framework None
? Choose rendering mode Universal (SSR)
? Choose development tools (Press <space> to select, <a> to toggle all, <i> to invert selection)
yarn run v1.21.1
$ eslint --ext .js,.vue --ignore-path .gitignore . --fix
✨  Done in 0.87s.

🎉  Successfully created project nuxt-linting
```
Notice the configuration runs `eslint --fix` at the end to correct any errors based on the default eslint and prettier configuration. Change to the project directory and start the dev server: `yarn dev` or `npm run dev`. When you open the project on the browser it opens with no errors.

## Errors Arrive

Open the project in Webstorm., open `src/pages/index.vue`, and Reformat Code from the Code menu. Start the dev server: `yarn dev` and we have at least 36 errors.

Okay, so first tweak: `Settings/Editor/Code Style/HTML` in the `Other` tab, select the "In Empty Tag" checkbox. This adds a space around self closing tags `<logo />`

Next change. Set up Webstorm to use the project EsLint. Go to `Settings/Languages & Frameworks/Esling` and select "Manual ESLint configuration". In the ESLint package field select the the package from the project `node_modules`. Next manually select the project's `.eslintrc.js` file. This does not help the errors but eventually will.

### Prettier

Most of the errors are a result of a long standing issue with Prettier and Vuejs. The Vuejs standard is to not indent the style and script section of the Vue Template file. For a long time this was one of the most requested new features requested for Prettier but since version 1.19.0 it is now an option.

Upgrade to the latest Prettier: `yarn add prettier@1.19.1`. This will not fix any thing immediately. You need to add the following to the `prettierrc`conficuration file:

```javascript
"vueIndentScriptAndStyle": true
```

This one change reduces the errors from `35`in `index.vue` to just **4** errors!

Next, go back to `Settings/Editor/Code Style/HTML` in the "Tabs and Indent" tab and change "Continuation indent" to 2. NOW all the errors are down to one error `index.vue`.

This final error has to do with the wrapping of the fonts specified in the style section of `index.vue`and the only way I was able to fix this error was using `eslint --fix`. Overall I am well please with the outcome. In most project I develop my global styles via SASS so I will not use these styles in this file anyway.

I hope this has been helpful to you. Happy coding.
