---
title: iOS Versioning
layout: post
date: 2017-10-09
---

### The System

* Semantic Versioning: 
    - [Semantic Versioning](http://semver.org)(semver) promises a no-thought-required method of moving between versions. The format is `major.minor.patch` where `major` changes break public API, `minor` changes modify the API in a backwards-compatible manner, and `patch` changes leave the API unchanged. 
* Romantic Versioning: 
    - based on conceptual changes and still follows some similar ordinal pattern. 

### The Audience

The system we use goes hand-in-hand with *who* we are versioning for. Possible answers include the End-Users, Installer-Users and Internal-Users. 

* End-Users: People tapping on maps and actually using the application for its intended purpose. 
* Installer-Users: People installing the application to the device. 
* Internal-Users: Really just my friends and I.


### The Implementation 

#### (Apple's) Version Numbers and Build Numbers

From [Apple's official notes](https://developer.apple.com/library/content/technotes/tn2420/_index.html) and these SO Answers: [(1)](https://stackoverflow.com/a/38009895), [(2)](https://stackoverflow.com/a/6965086), we can pull out the following.

Let $B$ be build numbers and $V$ be version numbers.

* Def. Version Number:
    - String in the matching `\d(\.\d)*` with less than 18 characters.
    - Has key `CFBundleShortVersionString` in `Info.plist`
    - Call this set $Versions$.
* Def. Build Number:
    - String matching `\d(\.\d)?(\.\d)?`.
    - Has key `CFBundleVersion` in `Info.plist`
* Def. Release Train for $v \in V$: 
    - Set of submitted builds with version $v$. 
* Def. Apples Order:
    - $V \tims B$ is alpha ordered where $V,B$ have the natural order. 

### Concerning Coffeefilter

#### Versioning 

In this case I'll do Romantic/Internal as follows:

`major` for iOS versions, `minor` for anything that affects the way End-Users interact with the application, and `patch` for indirect changes. 

I'll use the [versioning](https://github.com/SiarheiFedartsou/fastlane-plugin-versioning) fastlane plugin when I need to edit the version 

#### Build Number 

While I'd love to encode extra data in the number, e.g. [twitch's time/commit encoding](https://blog.twitch.tv/ios-versioning-89e02f0a5146), I've decided on something simple for now:

Build numbers are only changed on via [Pilot](https://docs.fastlane.tools/actions/pilot/) on TestFlight uploads. 