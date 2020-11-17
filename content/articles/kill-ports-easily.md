---
title: 'Kill Blocked Ports'
date: 2019-07-22
published: true
tags: ['Gatsby', 'React', 'Commandline']
series: false
cover_image: images/blocked.jpg
canonical_url: false
description: Have you tried to start a development server and the port is blocked? This post offers a dynamic shell script to fix this problem quickly.
---

So you are working hard on your new [Gatsby](https://www.gatsbyjs.org/) site and fire up the development server.

**FAIL!**. You are presented with the console error message:

```bash
Something is already running at port 8000
Would you like to run the app at another port instead? [Y/n]
```

The cause is that a process did not fully close, or you terminated a terminal window without exiting the command.

## What To Do

_FYI. The fix below is geared toward MacOS or Ubuntu/Linux. It might work on Windows._

The workaround is simple.

- Exit the startup.
- Enter the following at the command prompt:

`kill -9 $(lsof -t -i:8000)`

Cool all fixed. But, do you want to search out this command every time this happens?

**NO**, of course not.

**Solution**: Create a command alias in your favorite terminal. In [ZSH](https://ohmyz.sh/) open your `zshrc` file and add the following alias:
`alias k8="kill -9 $(lsof -t -i:8000)"`

Now, the next time this happens, escape out of the develop script and enter `k8`. All done.

## All fixed, sort of.

The alias command only responds to one situation, and one port number. What about the Gatsby serve command (port 9000), or Create-React-App (port 3000)? You will have to create an alias for each situation. There has to be a more productive way.

The answer is to create a shell script.
Create the below script:

```bash
#!/bin/bash

#styles
VP_RED='\033[01;31m'

# Update default core install
echo -e "${VP_RED}KILLING the SPECIFIED PORT"
kill $(lsof -t -i:$1)
```

A couple of note:

- The styles line `VP_RED='\033[01;31m'` only makes the command red in the terminal window.
- The `echo` line initiates the style.
- The most import part is the last line, which is the command that kills the port.
- The name of the file will be the command you type. In my case, `kport`.
- Make this file executable: `chmod +x kport`
- Place in the user's path.

In my case, I have added to `/bin` and named the file `kport`.
So, when you execute the file, remember to include a port number which you want to kill, as an argument (i.e.):
`kport 8000`

DONE! I hope this helps. Have a great day.
