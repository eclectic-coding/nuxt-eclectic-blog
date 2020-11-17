---
title: System Automation
date: 2020-06-21
published: true
tags: ['html', 'javascript', 'beginners', 'tutorials']
series: false
cover_image: images/automate.jpg
canonical_rul: false
description: So I have been writing about streaming our workflow, but this does not have to be about coding. How about our productivity on our systems? In this article I focusing on scheduling some jobs on Unix based systems, mostly centered around securing data.
---
So I have been writing about streaming our workflow, but this does not have to be about coding. How about our productivity on our systems? In this article I focusing on scheduling some jobs on Unix based systems, mostly centered around securing data.

## Schedule Jobs with Cron
What in the world is Cron? Well: "...cron is a time-based job scheduler in Unix-like computer operating systems. Users that set up and maintain software environments use cron to schedule jobs to run periodically at fixed times, dates, or intervals."

A streamlined workflow may include many scheduled jobs to manage a user system. I am going to touch on three jobs I have on my system:
- Backups
- Empty Trashcan
- Backup my cron jobs.

So, we use the default editor for `cron`, to edit of schedules: `crontab -e`. If you are interested, I suggest you read this [tutorial](https://code.tutsplus.com/tutorials/scheduling-tasks-with-cron-jobs--net-8800) on the scheduling syntax.

## Backups
I am pathological about daily backups. I was a small business owner for twenty years and data is a product of work and therefore an investment. So, lost data represents lost money.

**My System**: I have a Linux laptop, with a separate drive for my home directory. On a Linux systems, this includes all your personal files and your configurations of most programs or packages you install. If you reinstall the operating system, this makes it easy, because all of your files are separate from the operating system. I have a second mass storage drive where I keep large files: videos, downloads, VM's.

So, I backup my home drive every evening at 11:30pm. We open `crontab -e` and add the following:
```
# Daily System backup using RSYNC
30 23 * * * rsync --archive --delete --progress --human-readable --exclude-from '~/bin/rsync-exclude-list.txt' ~/ /media/username/Backup/home-backup | tee ~/rsync.log
```
I backup my storage drive on Saturday evenings:
```
# Weekly backup of Storage Drive
30 20 * * 6 rsync --archive --delete --progress --human-readable /media/storage /media/webrev/Backup/storage-drive | tee ~rsync-storage.log
```
Notice the `6` at the beginning of the line? Saturday is represented as a `6`.

## Trashcan
What about the trashcan, also know as the recycle bin? Do you ever remember to empty it? I have created a `cron` job so I do not have to worry about it.

**trash-cli**: There is a nice little package called [Trash-cli](https://github.com/andreafrancia/trash-cli) I have installed. It works on most Linux systems. Install on on Ubuntu with `sudo apt install trash-cli`. Refer to the [repo](https://github.com/andreafrancia/trash-cli) for more details.

Add to our `cron` jobs to run on the first day of the month:
```
# Empty trash monthly - first day of the month 12AM
0 0 1 * * /usr/bin/trash-empty
```

## Backup Crontab
We are running a backup of the home directory, but our `cron` configuration is in the operating system partition and if we reinstall the OS, our configuration will be lost. The command `crontab -l` will output the crontab configuration. We can easily automate this task, and save to our home directory which is backed up separately:
```
# Backup my crontab file - second day of the month 12AM
0 0 2 * * /usr/bin/crontab -l > ~my_crontab.backup
```

What other automation jobs are you runnning? Send me a message and let me know.
