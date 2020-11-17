---
title: 'Linux Development at Flatiron'
date: 2019-09-19
published: true
tags: ['SQlite','Node','Linux','Postgres']
series: false
cover_image: images/linux-banner.jpg
canonical_url: false
description: This article walks you through my environment setup while I was attending Flatiron School Bootcamp in Software Engineering.
---

## Introduction
I have dabbled in Linux for years and I want to use my new System76 laptop to code. Below is the workaround I have developed for the current Learn environment used by Flatiron School.

At the beginning of this article, there are a few parameters which need to be defined at the outset.
- The assumption is that you have a working knowledge of your computer and are comfortable with the terminal (a.k.a command line).
- This article will be following Learn.co's tutorial Manual Environment Set Up, with changes for the Linux platform.
- The third assumption it that you have GIT installed and configured. If not, here is site that can help you get started.

## Command line tools
To begin, if you have installed the Learn IDE, you need to uninstall before you begin setting up this environment (Deleting Learn IDE). Also, you will not need to install Homebrew or Xcode, as these package environments are for Mac only. The packages installed by these environments are normally available on Linux by default or are available for installation separately.

There is one support library which you will need to install: GnuPG. To check if it is installed, at the command line, issue this command: `gnupg --version`.

RedHat based systems using dnf/yum/rpm based systems `sudo dnf install gnupg`

Debian based systems using APT package managers
`sudo apt-get install gnupg`

## Ruby
Before you install Ruby, you need to make a architecture decision.
- Is your computer a single user system?
- Do you need Ruby installed globally?
- Or just for the single user account?

I decided for the later approach, as its easier to install and manage RVM with the single user setup. You will need to uninstall Ruby if it is installed globally. Since, the installation is well documented on the RVM site, I went with these directions. First make sure you have `curl` installed `curl --version`. If not:

dnf/yum/rpm based systems
`sudo dnf install curl`

APT package managers
`sudo apt-get install curl`

Installing RVM requires two platform independent commands:

Install GPG keys:
`gpg2 — recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB`

Install RVM:
`curl -sSL https://get.rvm.io | bash -s stable`

The RVM web site documents that you can install Rails at the same time as RVM but you should wait until we set up a few more packages.

### Setup
You can issue the command `rvm list known` to view the available versions. To install Ruby: `rvm install 2.6.3`. Then to use the new install version of Ruby `rvm use 2.6.3`. You can manage multiple version of Ruby so you probably want to set a default version: `rvm --default use 2.6.3`.



<div style="width: 150px; height: auto" >
    ![rubys](../../assets/images/rubys.jpg)
</div>

### Ruby gems
Ruby gems are basically a library of code, which are easily accessible to you.
1. Update the gem system: `gem update system`.
2. Install Learn gems. This is a lengthy install, so enjoy a cup of coffee ☕️ `gem install learn-co`.
3. Install bundler gem: `gem install bundler`.
4. Install nokogiri with: `gem install nokogiri`. According to the Learn website: “Nokogiri is a gem to help parse HTML — useful when we want to scrape websites.” This gem install worked on my Fedora system without any issues, but if you encounter problems, Nokogiri documentation offers a complete troubleshooting guide. I have installed successfully on MAC, Ubuntu, POP_OS, and Fedora without any breaking issues.

