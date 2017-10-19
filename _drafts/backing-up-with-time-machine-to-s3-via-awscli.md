---
title: Backing Up with Time Machine to S3 via awscli
layout: post
date: 2017-10-18
---

## Why 

I have a lot of data. We all do. I also have a limited amount of resources in cash/time. So I want a simple/automated/low-cost backup system. After looking around I found few possibly good routes. There were several cloud-first services that looked great, but were out of my price range. Arq seemed especially good. The popular Time Machine alternatives, Super Duper and Carbon Copy Cloner, were also out of reach. 

In the end I decided on Time Machine + S3. 

## Making Space 

First, we need to decide on the content of our backups. I used [DaisyDisk](https://daisydiskapp.com) (free) to estimate the size of my backups. They have a *collection* where you can drag graph elements. Extremely helpful. My initial backup size came in at 40gb after some [exclusions](http://pondini.org/TM/11.html). 

Second, we need to prepare a local place for our backups. I have a `64gb` "backup" volume on a partitioned [Samsung key](https://www.amazon.com/gp/product/B017DH3O5A/). Make sure the partition map is `GUID` and the volume is `hfs+`. Also disable indexing on this drive, else we'll get copies of 

## Time Machine

Time Machine has been around since 10.5, very distinguished. 

### Before Backup
It's a good idea to cleanup your package managers before starting. e.g. use `brew cleanup` and `gem cleanup`. I had a large amount of gem docs hanging around. So I [removed them](https://stackoverflow.com/questions/2941005/how-to-remove-installed-ri-and-rdoc/2944084#2944084) and [kept them from being installed](https://stackoverflow.com/questions/1381725/how-to-make-no-ri-no-rdoc-the-default-for-gem-install).

### During Backup 

Time Machine is Throttled by default, which is understandable. Usually the backup drive almost always connected, and things can be run over night. In my case, I plan to insert my drive almost exclusively for backups. So I'll turn off throttling via `sudo sysctl debug.lowpri_throttle_enabled=0`. This setting will revert itself on reboot, or you can enter `sudo sysctl debug.lowpri_throttle_enabled=1`. 

In case you get antsy during the process, you can watch every file with `sudo fs_usage backupd`. Where `backupd` is the demon behind Time Machine.

### After Backup 
In 10.13, time machine snapshots [have changed](https://eclecticlight.co/2017/06/22/mobile-time-machine-and-its-transformation-in-high-sierra/) for the better. Now apple uses [APFS Snapshots](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html#//apple_ref/doc/uid/TP40016999-CH5-DontLinkElementID_5) under the hood, which use less space than their previous implementation. But, if you need more space, you can delete all mobile snapshots via `tmutil thinlocalsnapshots / 1 4`. 

## S3 

We will use the `aws s3 sync` for our uploading. 

## More Information  

* Tutorials 
    - [Time Machine without Time Capsule](https://superuser.com/questions/33885/wireless-backups-with-time-machine-without-time-capsule)
    - [auto mount/unmount time machine disk](https://somethinginteractive.com/blog/2013/07/24/time-machine-auto-mountunmount-drive-os-x/)
    - [installing s3fs](https://cloud.netapp.com/blog/amazon-s3-as-a-file-system)
    - [glacier backup](https://lifehacker.com/how-to-use-amazon-glacier-as-a-dirt-cheap-backup-solut-1460814873)
    - [s3 backup via cli](https://aws.amazon.com/getting-started/tutorials/backup-to-s3-cli/)
* Tools 
    - aws
        - [s3 sync](http://docs.aws.amazon.com/cli/latest/reference/s3/sync.html#)
        - [s3ql](https://bitbucket.org/nikratio/s3ql/)
        - [s3cmd](http://s3tools.org/s3cmd)
        - [s3fs](https://github.com/s3fs-fuse/s3fs-fuse)
        - [s3backer](https://github.com/archiecobbs/s3backer)
        - [aws glacier](https://aws.amazon.com/glacier/)
        - [aws simple monthly calculator](https://calculator.s3.amazonaws.com/index.html)
    - Standard GUI
        - [Carbon Copy Cloner](https://bombich.com)
        - [SuperDuper](http://www.shirt-pocket.com/SuperDuper/SuperDuperDescription.html)
    - cli
        - [git-annex](https://git-annex.branchable.com)
    - Time Machine
        - [Time Machine](https://en.wikipedia.org/wiki/Time_Machine_(macOS))
        - [tmutil](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man8/tmutil.8.html)
* Services 
    - [CloudBerry](https://www.cloudberrylab.com)
    - [DollyDrive](http://www.dollydrive.com)
    - [Arq](https://www.arqbackup.com)
    - [Backblaze](https://www.backblaze.com)
    - [CrashPlan](https://www.crashplan.com/en-us/)
* Stack Exchange
    - [s3fs + time machine](https://apple.stackexchange.com/a/7003)
    - [s3ql + netatalk](https://apple.stackexchange.com/a/51634)
    - [eject backup drive automatically](https://apple.stackexchange.com/questions/800/how-do-i-eject-the-time-machine-backup-drive-automatically-after-each-backup)
    - [stop indexing drives](https://apple.stackexchange.com/questions/29580/how-do-i-make-spotlight-stop-indexing-my-backup-drive)
* Concerning Time Machine 
    - [FAQ](http://pondini.org/TM/FAQ.html)
    - [Exclusions](https://apple.stackexchange.com/a/25833)
    - [downside of Time Machine](https://apple.stackexchange.com/questions/24667/what-are-the-downsides-to-using-time-machine)
    - [time machine exclusions](https://apple.stackexchange.com/questions/131399/what-folders-can-be-safely-excluded-from-time-machine-backup)
    - [on encryption](https://apple.stackexchange.com/questions/81460/should-time-machine-backups-be-encrypted)
    - [time machine restoration](https://apple.stackexchange.com/questions/490/how-do-i-use-time-machine-to-restore-my-entire-computer-rather-than-a-file)
    - Throttling
        - [SE](https://apple.stackexchange.com/questions/212537/time-machine-ridiculously-slow-after-el-capitan-upgrade)
        - [reddit](https://www.reddit.com/r/apple/comments/5vb66w/speed_up_your_time_machine_backups_tremendously/?st=j8xved1v&sh=7fe82e25)
