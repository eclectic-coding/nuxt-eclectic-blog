---
title: 'TODO list in VSCode'
date: 2019-08-14
published: true
tags: ['VSCode','Productivity']
series: false
cover_image: images/checklist.jpg
canonical_url: false
description: This article addresses a document level TODO list as your are coding and developing an application.
---
_This article was inspired by a thread on [Twitter](https://twitter.com/thecaitcode/status/1160348409313652736) started by [Caitlyn Greffly](https://caitlyngreffly.com/)_

So, the problem is how do you manage your TODO list as you are coding? You are working on basic functionality in a component and having a working plan, either in a working flow diagram or a written list. How do you have those mental notes of TODO's? There are a few simple options:
- A handwritten list (who writes anymore?)
- A Markdown file in the project directory
- A Trello of Board is a good visual reminder, especially when coupled with the Trello mobile app.

## VSCode Extensions
If you are using VSCode as your IDE, you already know there are a ton of extensions for almost everything you need. For a few months now, I have been using an extension called [TODO Tree](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree) which displays a tree view in the explorer pane of VSCode. Clicking a TODO within the tree will open the file and put the cursor on the line containing the TODO.

Here is how it basically works. Install the extension from the link above and test it out by adding a TODO to your source code like this:
`// TODO - todo note`

or a FIXME note
`// FIXME - fix something`

Now let's explore the interface

## Interface
![GIF of the Toolbox in Action.](https://thepracticaldev.s3.amazonaws.com/i/uw5odwnsgfep9d56iczc.gif)

Notice in the sidebar you now have a box for TODO Tree. There are several icons (see above) which give you different options to filter and display the list of TODO's or FIXME notes. When you click on any of these notes the source file will automatically open.

## Highlight
One of the nicest features, in my option, is the ability to highlight the notes in the source code and color code the icons in the TODO Tree. This feature is well documented in the source code and I have provided my settings below. Just copy to your VSCode settings.json:

```json
  "todo-tree.customHighlight": {
    "TODO": {
      "icon": "check",
      "type": "line",
      "iconColour": "yellow",
      "foreground": "red",
      "background": "yellow",
    },
    "FIXME": {
      "icon": "beaker",
      "iconColour": "red",
      "foreground": "white",
      "background": "red"
    }
  },
```
Here is the result:

![Screenshot.](https://thepracticaldev.s3.amazonaws.com/i/vporbgidpzomglepojz2.png)

Give TODO Tree a try and if I can help with your configuration message me.
