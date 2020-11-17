---
title: GIT Commit
date: 2020-04-21
published: true
tags: ['GIT', 'Coding', 'Helping']
cover_image: images/support-letters.jpg
canonical_url: false
description: Contributing on open source projects can be fun. However, if you have never made a pull request on GitHub, it can be unnerving. I am sharing my experiences and a short list of how to get started.
---
Contributing on open source projects can be fun. However, if you have never made a pull request on GitHub, it can be unnerving. I am sharing my experiences and a short list of how to get started.

## Overview
As most developers know, a GIT repository provider supports a community where millions of people learn, share, and work together to build software. These providers are built using a version of [git][GIT], an open source distributed version control system. There are several providers, namely, [GitHub][GITHUB], [Gitlab][GITLAB], and [Bitbucket][BIT]. Although I have used all three service providers, I will be referring in this article to **GitHub** as it represents a larger market share.

## My first experience
I have been a coding hobbyist for years. You can read more of my journey here: [My Story][PASSION]. Even with a desire to learn, I have discovered, one of the hardest parts of starting to contributing is **starting**. In my case, I had taken some online tutorials at [KnowTheCode][KTC] that were interactive, and I knew the overall process. At the time I was developing WordPress sites and downloaded a Genesis template to try. The install script did not work for me and I found the problem in the source code. I knew this would be a problem for others so I opened an issue at the GitHub repository. The developer was very kind, thanked me, and told me he would fix the problem, but encouraged me to read his **CONTRIBUTING** page, which was very detailed. He even listed me as a CONTRIBUTOR, which was very encouraging. So how do you get started?

## How do you learn
I will cover a few basics below, but it you are not familiar with the Git Workflow, it would be important to work through a tutorial to learn your way around the process. Here are a few suggestions:
- [Git 2.5, including multiple worktrees and triangular workflows][WORKFLOW]
- This a descent interactive [Tutorial][TUTORIAL] by GitHub

## Where?
I am asked many times by new developers: "Where do you contribute?" I can easily answer this with another question: "What software packages do you use?" I am not trying to be cavalier, but it can be that simple. Let me share an example for my own journey.

When I first started learning React, I used Bootstrap for styling so I could focus on learning React. I had used Bootstrap enough to recognize that the documentation in [Create React App's][CRA] documentation was incomplete. So I submitted a pull request.

So, "Where do you contribute?" Starting by looking at rough spots in the software in which you use. If you are a new developer, look at the documentation with *fresh eyes* and get started supporting open source software. This in invaluable experience as you move forward into a development career.

## Basics
To contribute to a project you need to become familiar with the **Triangle Workflow**

![Git Triangle Workflow Image.](images/triangle-workflow.png)
*Credit*: [Git 2.5, including multiple worktrees and triangular workflows][WORKFLOW]

The basics are as follows:
- Fork the repository you will contribute to your GitHub account.
- Clone *your fork* of the repository to your local computer.
- Make a new descriptive branch to work on (see document below).
- Commit changes to your local branch and push to your repository.
- Go to your repository on GitHub and create a pull request.

These are only the basics. The GitHub tutorial mentioned above is a good interactive example to get you use to the process. Also, I use a CONTRIBTUING document on most of my repositories which I downloaded from the first project I contributed. I continue to develop this document but fork this Gist as a reference and learn: [Sample CONTRIBUTING.md][CONTRIBUTING].

How do you contribute? Just get started.

[GIT]: https://git-scm.com/
[GITHUB]: https://github.com
[GITLAB]: https://gitlab.com
[BIT]: https://bitbucket.com
[PASSION]: ./passionate-journey.md
[KTC]: https://knowthecode.io/
[WORKFLOW]: https://github.blog/2015-07-29-git-2-5-including-multiple-worktrees-and-triangular-workflows/
[TUTORIAL]: https://firstcontributions.github.io/
[CRA]: https://create-react-app.dev/docs/adding-bootstrap/#using-a-custom-theme
[CONTRIBUTING]: https://gist.github.com/eclectic-coding/5815b6823991c2bf9b497da43897e061
