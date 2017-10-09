---
layout: post
title: "How I made this site (or how to follow several tutorials)"
date: 2017-10-08
---

## Separation of Concerns 

I made a distinction between *content* and *implementation*, as one does. 

### Content

I had two purposes. 

* (1) I want to be trusted. 
* (2) I need discourse. 

For (1), people on the internet are scary. You can't know anything *for sure* and it's even worse when a search turns up nothing at all. 
For (2), I've taken to commenting to myself in private github projects of late. That's definitely now what it was made for, and, it is, in fact, depressing. 

So I came up with the following content list: 

* Proofs for Paolo Aluffi's [Algebra: Chapter 0](https://www.maa.org/press/maa-reviews/algebra-chapter-0)
    - I started this in undergrad for an introduction to Category Theory and it needs to be finished. 
* Tutorials
    - I didn't remember how I setup my first droplet, until I did it again. 
* A Schedule 
    - It's like being publicly required to follow through. 
* Notes 
    - I *mean* to follow up on podcasts and dwell on good scenes, but I don't. 
* Reactions 
    - Reading peoples reactions to anime/books on Reddit is *nice*, but participating in some fashion would be even *better*.

### Implementation 

Initially I wanted something slightly grand in a language/framework I've never used. Ruby/Rails fit the bill. I repented as soon as I considered a database of blog posts. 

I've been writing in Markdown since junior year. I'm fairly certain it was the first *language* I ever touched. With that in mind, my goals for an implementation were the following:

* Easy content via markdown
* editable from iOS 
* a trustworthy domain
* some `ruby`, somewhere
* inline latex

[Jekyll](https://jekyllrb.com/) was exactly what I wanted to generate a site. It's written in (and extensible by) ruby + turns markdown into something serve-able. As a plus, I followed [this tutorial](http://jakub.fedyczak.net/post/editing-jekyll-site-on-ios/) for editing via iOS. 

It turns out hosting is expensive. It also turns out companies like a good user base, which is why I still have leftover credits from [github's student pack](https://education.github.com/pack). My main choice was `ajcaldwell.io`, since `ajcaldwell.com` has been taken for several years *and* it's only $40 on [Route53](https://aws.amazon.com/route53/). But, AWS was smart and won't let you exchange credit for something of real value. That's why I'm sticking with `tomatrow.github.io` in the meantime.
