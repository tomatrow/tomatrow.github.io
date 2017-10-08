---
layout: post
title: "Initial Blog Setup"
date: 2017-10-07
---

# Planning Stage

## Options 

### Content 

* About
    - Def. *tomorrow* : "on the day after today"
    - Def. *tomatrow* : "misspelling / my handle"
* Weekly
    - Describes reoccurring events with name/purpose/location/startTime/endTime. 
    + Ex. Church: SundayService, YAMondays, Weekend/Midweek JrHigh, Friday JrHigh SmallGroups
    + Ex. Anime: Fate/Apocrypha Monday 
    + Ex. Reading: Oathbringer Tuesday
* Resume
    - Just pull out of resume.json
* Media
    - anime 
    - novels 
    - short stories 
    - serials
    - music
* Math
    - Category Theory 
        + chapter zero
    - Type Theory 
    - Algebra
        + D&F
* Dev
    - tutorials
    - notes
        + podcasts
        + docs
    - setups 
    - reviews 
        + tools 
        + languages 

### Implementation

* host
    - github pages
    - digital ocean 
    - aws
    - heroku
* domain (via Namecheap through the github-student-pack)
    - optionaldefault.me
    - *some play on my name*.me
* backend
    - ruby/rails
    - swift/vapor
    - swift/perfect
    - php/laravel
    - js/node?
    - markdown/jekyll
* frontend
    - complicated
        + react
        + markdown (with latex extensions) -> redcarpet
    - simple 
        + use what's already given / most natural 
        + pandoc with `tex_math_dollars`
* look
    - mobile friendly
        - guide on top
        - idenity in middle
            + username
                + links to home
            + real-name subtext
        - content on bottom 


## choice
* Concerning content: 
    - dev: 
        + Notes on documentation I've read.
        + Notes on technical podcasts I've listened to.
        + Notes on technical articles I've read.
        + Interesting solutions from personal projects.
    - media: 
        + Notes/Reactions/Reviews on chunks of novel/short-story reading. 
        + Weekly pre/post notes on anime airings.  
    - math:
        + Notes on D&F.
        + Solutions for D&F. 
* Concerning implementation: 
    - host
        + I'll use Digital Ocean
    - domain
        + I'll just use `tomatrow.github.io`, that is *my* name.
    - backend
        + jekyll/markdown following [jakub](http://jakub.fedyczak.net/post/editing-jekyll-site-on-ios/) so I can edit on iOS. 
    - frontend


## Jekell Examples 

* [Asian Fun](http://robotkang.cc/)
* [3-most-recent + my-choice-look](http://www.anilwadghule.com)
* [too-simple](http://thejqr.com)
* [good-colors + simplicity](https://tatey.com)
* [top-links + simple-list](http://jasonrudolph.com)
* [too-simple?](http://appden.com)
* [project-page](http://claudiob.github.io)
* [top-links + plain](http://olesenm.github.io)
* [simple yet complicated](http://zigzag.github.io)
* [cool-color/favicon and layout](http://alexbcoles.com)
* [pleasing colors](http://martinisoftware.com)
* [best-layout](https://ganesshkumar.com)
* [also-good-layout](https://www.windespair.com)
* [good-reactive-layout](https://lightrains.com)
* [cute-orange](https://imprashant.com)
* [unique](https://diegosc.com)
* [cool + clean](http://yateender.com)
* [asian shinanigans](http://daodaoliang.com)
* [developer of ios+pods](https://kaunteya.github.io)

## Road to 1.0

- [ ] follow [jakub](http://jakub.fedyczak.net/post/editing-jekyll-site-on-ios/) though to the end
- [ ] add main page 
- [ ] add about
- [ ] add resume 
- [ ] enable markdown with latex extensions 
- [ ] add weekly 
- [ ] add posts 




# Log

## Users
- [x] Add a sudo user: [guide](https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart)
- [x] make it the default shell via [chsh](https://askubuntu.com/a/87858)

## DNS
- [x] connect namecheap to digital-ocean [guide](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars#registrar-namecheap)
- [x] assocciate digital-ocean with my domain via [console](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-host-name-with-digitalocean)

## ssh
- [x] add my public mac's key to droplet via [guide](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2)
- [x] remove password authentication via [guide](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04)
- [x] generate a key for tomatrow@droplet00 via `ssh-keygen` with default values. 
- [x] add `tomatrow@droplet00` key to github
- [x] generate key or Coda on iOS
- [x] copy key to droplet

## Installations 
- [x] `fish` via [subscription](https://launchpad.net/~fish-shell/+archive/ubuntu/release-2)
- [x] `micro` via [snap](https://snapcraft.io)
    - need to use `sudo` to run it...
- [x] packages for coda/jekyll workflow accoriding to the tutorial 
    - `apt-get install git jekyll jekyll-from-redirect ruby ruby-dev make gcc nodejs`
    - seems the guide is slightly outdated, `jekyll-from-redirect` is now [`jekyll-redirect-from`](https://github.com/jekyll/jekyll-redirect-from)
    - Used: `sudo apt-get install git jekyll ruby ruby-dev make gcc nodejs`
- [x] install exa via binary
    - had to manually install ``

## shell
* [x] shorten prompt to `/a/b/c/current_directory>` via `fish_prompt`

## blog 
- [x] follow [guide](http://jmcglone.com/guides/github-pages/) for my inital jekyll
- [x] serve via `cd ~/projects/tomatrow.github.io; and jekyll serve --host=optionaldefault.me` on droplet00 after following [guide](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-jekyll-development-site-on-ubuntu-16-04) 
- [ ] add checkboxs to kramdown via liquid using [guide](http://blog.winddweb.info/implement-github-like-checkbox)
