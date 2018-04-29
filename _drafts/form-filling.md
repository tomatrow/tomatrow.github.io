# An Application of Form Filling 

## The Problem 

We want to fill out forms. Specifically; we want to fill out forms for contracts between companies (procurement). 

The easiest way to do this is with browser extensions. At the core, I need two things: 

* __a__: A way to enter data about a company. 
* __b__: A map from data paths to form fields. 

The most basic implementation of __a,b__ is a `yaml` structure for company data and a sequence of (url, css selector, data path) to completely determine which form fields to fill out.

## Routes 

There are two routes we can take. 

There is a divide between Safari and the rest of the major browsers where extensions are concerned. 

There exists [a general specification](https://browserext.github.io/browserext/) based on Chrome's extension system. Each major browser except Safari aims to follow this spec. Apple, however, has their own unique extension system. 

## Choice

Both extension systems will work. Since we want easy/fast first, I'll use Safari for it's familiarity.