---
title: The Coffeehouse Schema
layout: post
---




## Links 

* XML Parsing 
    - [Graphical xpath](http://www.qutoric.com/xslt/analyser/xpathtool.html)
    - [xmlstarlet](http://xmlstar.sourceforge.net)
* Our data
    - [The Map](https://www.google.com/maps/d/u/0/viewer?mid=1w3GLW8rs_d9xTbU8nQ52Bf5xwGo&ll=32.94402489959027%2C-117.14390477500001&z=8)
* Unix tools I need to use 
    - [sed vs awk](https://stackoverflow.com/questions/1632113/what-is-the-difference-between-sed-and-awk)
    - [grep ses and awk](https://stackoverflow.com/questions/7727640/differences-among-grep-awk-and-sed)


## Parsing the Map 

The `xpath` expression `/kml/Document/Folder[name = "Coffee Shops"]/Placemark` gives us a list of `Placemark`s. Each `Placemark` has a `name`, `styleUrl`, and `Point` where `Point` has a single value `Coordinates`. A `Coordinates` is in the form `LATTITUDE, LONGITUDE, ?`. 

This would be perfectly fine if [namespaces]() didn't exist. 
So `xml sel -t -m '//_:Document/_:Folder[_:name = "Coffee Shops"]/_:Placemark' -c "." -n` *actually* works. See the [offcial troblueshooting guide](). 

To avoid the insanity above, delete the namespace and instead use `xml sel -t -m '/kml/Document/Folder[name = "Coffee Shops"]/Placemark' -c "." -n`


To get a big list of almost-what-we-want use 
`xml sel --text --noblanks -t -c '/kml/Document/Folder[name = "Coffee Shops"]/Placemark'`


* Steps 
    - Download the kml for mgoogle 
    - Remove the namespace via `xmlstarlet`
    - get the data into something I almost want
    - remove whitespace and separate on `#` via `sed` into something like `$name\n$colorcode\n$latitude,$longitude,$?\n`
    - pull out data via `awk` into `json`