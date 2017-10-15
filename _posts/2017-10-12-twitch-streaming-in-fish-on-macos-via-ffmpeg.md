---
title: Twitch Streaming in Fish on MacOS via FFmpeg
layout: post
date: 2017-10-12
---

## Intro

Everybody streams nowadays. Since JustinTV pivoted to Twitch, they've encouraged more content than gaming, including (but not limited to) art, programming, and collaborative efforts in the form of "twitch-*does-something*". 

So I've decided to try it out. 

At first I used [OBS](https://obsproject.com) to stream the time I work on [Coffeefilter](http://coffeefilter.online) (mostly reading documentation honestly), but coding/building/streaming-[realaxbeats](https://www.twitch.tv/relaxbeats) and trying to record the process turns my Air to ash. So, in aim of lighter resources and more control I'm going to start using [fish](https://fishshell.com)/[FFmpeg](https://www.ffmpeg.org). 

## Step by Step 

### Twitch Stream Key

If you're going to put yourself out there, it's a $\textit{good idea}^{TM}$ use two-factor-auth pn Twitch. 

So head over to [Twitch Dashboard](https://www.twitch.tv/tomatrow/dashboard/settings/streamkey) and get your personal stream-key. It looks like `live_XXXXXX_XXXXXXXX`.

Instead of just leaving that key viable in plain text on disk, store it securely in key-chain. Story your key in the login-keychain with the Name/Where fields as `twitch-stream-key`. We can access it via terminal via `security find-generic-password -a 'tomatrow' -s 'twitch-stream-key' -w` where you replace `tomatrow` with your own username. 

### Configuring Sound 

#### The Goal
Here's the ideal setup: 

* (1) Our desktop experience remains relatively the same. 
* (2) That experience is somewhat similar to what's presented on Twitch. 

#### The Tools 

To accomplish these goals we will be using `Soundflower` and `Audio MIDI Setup`. 

Soundflower is a kernel-extension that adds a virtual audio input/output device to your system. It has an interesting history. Written by [akhudek](https://github.com/akhudek/Soundflower), forked by [RogueAmoeba](https://github.com/RogueAmoeba/Soundflower-Original), and now (hopefully) maintained by [mattingalls](https://github.com/mattingalls/Soundflower). 

Audio MIDI Setup is a powerful application hidden in `/Applications/Utilities` that allows you to manage audio configurations.

#### The Implementation

Use `brew cask install Soundflower` to get 2.0b2. 

Restart. 

Open `Audio MIDI Setup` and use the plus button in the bottom-left-corner and select `Create Aggregate Device`. Name it `Twitch-Input` and check `Soundflower (2ch)` in the `Use` column. Next, create a new device using `Create Multi-Output Device`. Call it `Twitch-Output`. Select `Soundflower (2ch)` and `Built-In Output` to in the `Use` column. 

`Twitch-Input` is for use in the `fish` script, and `Twitch-Output` is the Output Device we use so that our system audio is sent through Soundflower *and* we can still hear it. Note your audio keys won't work when using `Twitch-Output`. 


#### In Case of Trouble 

* Case: "I can't hear anything": Try each one in order until it works, or just restart. 
    - (1) Switch output devices. 
    - (2) `kill` the coreaudio demon. 
        + `sudo killall coreaudiod` should do it. 
        + Thanks [Sbugurt](https://superuser.com/a/647556)
    - (3) Reload the audio kernel extentions. 
        + e.g. `sudo kextunload /System/Library/Extensions/AppleHDA.kext; and sudo kextload /System/Library/Extensions/AppleHDA.kext`
        + Thanks [ali](https://superuser.com/a/742984)

### Configuring FFmpeg

#### Installation 

I recommend installation of `ffmpeg` via `brew install ffmepg`. 

#### Device Info

You can see your own device list via `ffmpeg -f avfoundation -list_devices true -i ""`. 

Set `IN_VIDEO_ID` to the screen you want to capture and `IN_AUDIO_ID` to `Twitch-Output` which we set up previously. 

#### The Function

Save the following script to your `fish` functions directory, e.g. `~/.config/fish/functions`. 

<script src="https://gist.github.com/tomatrow/dbff88ce66d59c19f0b5331dd05e1a7c.js"></script>

* Change the `IN_RES` for your screen. 
* Change `IN_VIDEO_ID`/`IN_AUDIO_ID` according to your listed devices. 
* Mess with `IN_FPS`, `CBR_INT`, `QUALITY` to get the look you want. 
* Optionally uncomment the `-af $AUDIO_FILTER` portion at the end of `OUTPUT_AUDIO_OPTIONS` to filter out non-vocal frequencies. 


### Sources 

The script is based on [Streaming to twitch.tv](https://wiki.archlinux.org/index.php/Streaming_to_twitch.tv) at the Arch Wiki. 

I used the default global options from [ffscreencast](https://github.com/cytopia/ffscreencast)

[This article](https://marc.tv/tutorial-hd-pvr-twitch-mac-os-ffmpeg/) by Marc Tönsing was helpful. 

- Twitch 
    + [Inspector](https://inspector.twitch.tv)
    + [Broadcasting Guidelines](https://stream.twitch.tv)
    + [Broadcast Requirements](https://help.twitch.tv/customer/portal/)
- wiki
    + [RTMP Protocol](https://en.wikipedia.org/wiki/Real-Time_Messaging_Protocol)
- SO Answers concerning Audio Routing
    + [Initial Soundflower setup](https://apple.stackexchange.com/a/222402)
    + [Streaming to multiple channels via Soundflower](https://apple.stackexchange.com/a/54676)
    + [Amusing note on switching *to* pulseaudio *from* soundflower](https://askubuntu.com/a/602687)