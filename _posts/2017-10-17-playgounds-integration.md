---
title: Playgrounds Integration
layout: post
date: 2017-10-17
---

## Intro 

`XCPlayground` and its ilk have been around for at least two years. I've never really used them in a workflow. After listening to [a podcast](https://swiftcoders.podbean.com/e/63-ryan-nystrom-ios-engineer-at-instagram/) where [Ryan Nystrom](http://whoisryannystrom.com) explains how he uses Playgrounds in his app [GitHawk](http://githawk.com), I've decided to try it out. 


## Integration 

* Create a new playground via `File>New>Playground`. 
    - e.g. in `/Playgrounds/Misc.playground` where `/` is our project root.
* Create a new Target with the Cocoa Touch Framework template. 
    - Target Options: 
        + Note. Don't embed this in the application. 
        + Note. Choose a good Product Name. 
            * e.g. `coffeegrounds` 
* Add anything you want to access to your new target. 
    - e.g. I put in *everything* from my main target. 
    - Note. You *have to* use the `public` keyword for each declaration you want to make visible in the playgrounds target. 
* Concerning Cocoapods. 
    - If you are using pods, you'll need to edit your Podfile to account for the playgrounds target. See [the docs](https://guides.cocoapods.org/using/the-podfile.html) for exactness. 

## Further Reading 

* [K Harrison](https://useyourloaf.com/blog/adding-playgrounds-to-xcode-projects/) on playgrounds integration. 
* [`pod try` + playgrounds](https://github.com/neonichu/ThisCouldBeUsButYouPlaying)
