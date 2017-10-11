---
title: Indexing Coffeehouses on DynamoDB
layout: post
published: false
---

Choosing a *useful* Primary-Key means we will means: 
* (1) We have 5 *useful* Local Secondary Indexes
* (2) We have more room for Global Secondary Indexes

## On Indexing 
* See [Q: How does selection of primary key influence the scalability I can achieve?](https://aws.amazon.com/dynamodb/faqs/) 
1121asz 


## Methods 

### Method 00
- Give each house a integer identifier. 
- Successive shops are given a higher identifier. 

### Method 01
- We choose a partition-sort key based on location. 
- The partition key will be a *region-id* $\in \Z^\ge$. 
    + A region is semantic, but it requires a Center coordiante and a Span.
    + This Center should be special. 
        + e.g. population density, local university location, city center, coffeehouse density
    + The Span should determine a GeoBox that reoughly matches the semantic region. 
- The sort-key is a simple idea with a slightly complicated implementation. 
    + The sort-key is called the Relative Distance of the House to the Center. 
        * Lower is closer, higher is farther, exact distance between houses is irrelevant. 
        * The initial distance is evenly distributed by the initial number of houses. A more exact specification is below. 
            - Let $C$ be the center.
            - Let $m = 0$ and *the max* $= M \in \Z^+$ . 
            - Let $H$ be the set of houses and $S = [0,M]$. 
            - Let $n = |H| << M$ be the initial number of houses. 
            - Let $d:H \to \R^\ge$ map a house to it's distance from $C$. 
            - Let $<$ order $H = \{h_1,h_2,...,h_n\}$ by $d$. 
            - Let $I = \lfloor \frac{M}{n+1} \rfloor$ be the interval.
            - Let $p:H \to [0,M]$ map a house to its position be defined as $p:h_i \mapsto i \cdot I$. 
        * New houses are added by finding the two house distance's it falls between, then placing it in the middle. 
            * i.e. inserting $h_i$ between $h_{i-1},h_{i+1}$ would be $p(h_{i-1} + \lfloor \frac{p(h_{i+1}) - p(h_{i-1})}{2} \rfloor)$
    + Caveats: 
        * In the event two houses fall in the same region and are the same distance from the center (which is super edgy), we modify one house's coordiante slightly. 
- Local Indexes
    + Name
    + Rating
    + lattitude
    + longitude

### Method 02
* This is a compromise between method 00 and 01. The partition-key will still be a region as defined in 01. But the sort-key will just be a integer identifier as in 00. 
* I can still make 01 into a local index. 
