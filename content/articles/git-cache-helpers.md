---
title: GIT Cache Helpers
date: 2019-12-09
published: true
tags: ['GIT', 'Security']
series: 'GIT Authentication'
cover_image: images/git-security.jpg
canonical_url: false
description: This is a second article in a series called "Git Authentication*" . In the previous article, I shared how I troubleshooted an authentication error, while working on lab assignments for Flatiron School.
---
This is a followup article in a series called [Git Authentication](https://dev.to/eclecticcoding/git-authentication-errors-109a). In the previous article, I shared how I troubleshooted an authentication error, while working on lab assignments for Flatiron School.

**DISCLAIMER**: *This solution works for my system. I have a single user system not connected to a work network, just a home office. Please investigate the best solution for you and your own security.*

## Options
There are several options Git provides for credentials with the HTTPS protocol. In am escalating order of options:
- Cache credentials helper - [GIT Authentication Errors](https://dev.to/eclecticcoding/git-authentication-errors-109a)
- Store credentials helper
- Local OS keyring helpers - this articles purpose

### Caching credentials
This is an option I dismissed very quickly, as I was looking for a viable option for my single user computer. To be fair, I do need to introduce this feature as another options

**Details**
The good news is that caching is quite secure because it keeps data only in memory. Just remember, every time you open new session, you need to type your credentials again. Memory is purged after 900 seconds (15 min) by default, but it can be changed with optional timeout parameter.
```shell
git config --global credential.helper 'cache --timeout=300'
```

Read more from the [Official Documentation](https://git-scm.com/docs/git-credential-cache)

 ### More Secure Steps
After setting up a Credential Store, I became increasingly more uncomfortable with this method. Even though I use a single use computer, it is basically a plain text method. Therefore, it does not matter if you have a 32 character password with uppercase and special characters, it is still plain text.

**Details**
The directions here are based on a Ubuntu 19.10 system. There are other options for Mac and [Windows](https://github.com/Microsoft/Git-Credential-Manager-for-Mac-and-Linux). The basic idea is to use your Operating System's secure keyring.

Five years ago, the best way to store Git credentials on Linux was to use the GNOME Keyring (libgnome-keyring), but as it is specific to GNOME, it is deprecated since [January 2014](https://mail.gnome.org/archives/commits-list/2014-January/msg01585.html). For Git versions 2.11+ you should use credential helper based on [libsecret](https://wiki.gnome.org/Projects/Libsecret). Installation and configuration takes only four bash commands:

```shell
sudo apt-get install libsecret-1-0 libsecret-1-dev
cd /usr/share/doc/git/contrib/credential/libsecret
sudo make
git config --global credential.helper /usr/share/doc/git/contrib/credential/libsecret/git-credential-libsecret
```
All done! ✅
The next time you use git you’ll be asked for your username and password will be the last time on your device
