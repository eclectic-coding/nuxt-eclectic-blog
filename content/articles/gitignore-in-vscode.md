---
title: 'Gitignore in VSCode'
date: 2019-07-25
published: true
tags: ['GIT', 'VSCode']
series: false
cover_image:
canonical_url: false
description: This week I published an article about automating the creation of LICENSE file for GIT in VSCODE. I wanted to consider the same methodology for the obligatory gitignore file.
---

This week I published an article about automating the creation of a [LICENSE file for GIT](https://dev.to/eclecticcoding/license-file-for-git-3dp) in VSCODE. I wanted to consider the same methodology for the obligatory `.gitignore` file.

As you probably already know, the `gitignore` file is used in your local GIT repository to ignore files, like `node_modules` directory and `env` files which may contain API keys, to exclude them from your repository.

Just like with the LICENSE file example, you could easily copy a `gitignore` file from another repository and include it in your next _commit_, but there is an **EASIER** way?

![alt text](../../static/images/vscode-marketplace.png "VSCODE Extensions Marketplace.")

##VSCODE Extensions
If you are using VSCODE as your IDE, you already know there are a ton of extensions for almost everything you need. Today, I found this nifty extension, called [.gitignore Generator](https://marketplace.visualstudio.com/items?itemName=piotrpalarz.vscode-gitignore-generator) which installs a `.gitignore file`, specific for your type of project, to your root project directory.

So, to use, go to the Command Palette, and type `gitignore`, and select `Generate .gitignore File.` A selection box will pop-up (see below):

![Screenshot of selection menu.](../../static/images/vscode-screenshot.png "Screenshot of selection menu.")

It will generate a `.gitignore` file based on the [gitignore](https://www.gitignore.io/) API.

Have a nice day.
