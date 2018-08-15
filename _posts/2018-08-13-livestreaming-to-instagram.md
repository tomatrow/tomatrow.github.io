---
layout: post
title: "Livestreaming to Instagram"
date: 2018-08-13
---

Instagram used to have a public API. That was removed at some point. So, people have discovered the private API by inspecting Instagram for iOS/Android.

The best library out there is [`mgp25`](<>)'s [`Instagram-API`](<>) which is written in [`php`](<>).

We're going to do the following:

-   (1) Install the prerequisite tools/libraries
-   (2) _Livestream_ a saved video

I'm assuming you're running a newer version of MacOS.

## The Process

### Prerequisite Installations

We will install:

-   (1) `homebrew`
-   (2) `composer`
-   (3) `Instagram-API`
-   (4) `liveBroadcast.php`


-   (1)
    Homebrew is a package manager for MacOS. We need it to install `composer`.

    Run `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"` in Terminal.

-   (2)
    [`composer`](<>) is a dependency manager for `php` applications.

    Run `brew install composer` in Terminal.

-   (3)
    [Instagram-API](<>) is the library we will be using.
    We will be running off master to get all the latest stuff.
    So, change directory to an empty folder.
        mkdir -p ~/sandbox/hesed/instagram
        cd ~/sandbox/hesed/instagram
        composer require mgp25/instagram-php dev-master
-   (4)
     Now we just move the example script from `Instagram-API`'s `examples` directory to the root of our project.    

        cp vendor/mgp25/instagram-php/examples/liveBroadcast.php .

Now, all files should be in place, and it should have the following structure:

    .
    ├── composer.json
    ├── composer.lock
    ├── liveBroadcast.php
    └── vendor

### Live-streaming

We will:

-   (1) change an import statement
-   (2) enter our username/passsword
-   (3) enter the video's location
-   (4) run it

Now, open up `liveBroadcast.php` in your favorite text editor.

-   (1)
    Since we moved `liveBroadcast.php`, we need to change an import statement:

        `require __DIR__.'/../vendor/autoload.php';`

    to

        `require __DIR__.'/vendor/autoload.php';`

-   (2)

## More Information
