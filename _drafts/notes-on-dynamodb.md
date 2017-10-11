---
title: Notes on DynamoDB
layout: post
---

## Links 
* [SOSP Paper](http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)
* [Dynamo FAQ](https://aws.amazon.com/dynamodb/faqs/)
* [Dynamo Docs](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)

## SOSP Paper

* It's always writable, pushing conflict resolution onto reads. 
* In the case of writing to the local store, and not being able to touch AWS, they use branching. Then we resolve the conflict. 

### Concerning parameters 

* Let R be the minimum number of nodes that must participate in a successful read operation. 
* Let W be the minimum number of nodes that must participate in a successful write operation. 
* Let N be the number of hosts. 
* `W = 1` $\thn$ a write is never rejected. 
* If `W,R` are low $\thn$ it's easy to read inconsistent data. 

### Use Cases
* Business logic specific reconciliation
    * We merge structures according to their purposes. 
* Timestamp based reconciliation
    - last write wins, super easy to do. 
* High performance read engine
    - `R = 1` and `W = N`



--- 


## FAQ 

### Dynamo as a service

#### Definition 
* It's a non-relational database separated from the hardware it runs on and the software that power it. 
* Concerning the hardware: Dynamo replicates data across separate location in a region. 

#### Cost/Limits
* In general, Request costs are low thanks to SSDs, but that also means storage costs are higher. 
* Eventually consistent reads cost more. 
* Items 
    * Number of Items are unlimited.
    * Size of Items are less than 400KB. 
    * Item attributes have no explicit limit. 

#### Purpose 
* It stores structured data, larges objects are better off in S3. 

### Data Structure 
* There are two options
    - (1) key-value store: Every Item/Attribute is assigned has a key and a value. The key is a string and the value is a type and the actual data. 
    - (2) document store: Instead of using the data structure AWS provides directly, we build one of the common specs on top of it. Namely JSON, XML, or HTML. 
* Using (2) with JSON, the API works pretty much the same, except with JSON. 


### API

#### Scanning 
* Gets the entire table, and can be filtered. 

#### Querying 
* Pulls items by their primary keys, or Global/Local indexes. 
* Know the definitions of the various keys and indexes. 

#### Updating 
* Note. Item upload rate is limited at 25/request.

#### Filtering
* The same idea as before, the dynamo provides [simple expressions](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.OperatorsAndFunctions.html#Expressions.OperatorsAndFunctions.Syntax)closer to the metal, and we can run though lambda for anything more complicated. 

### Indexes 

* Def. Partition-Key: 
    - A chosen attribute for each item whose hash determines it's placing in the database. 
* Def. Sort-Key: 
    - A chosen attribute which determines an a partial order over the set of Items. 
* Def. Primary-Key: 
    * A required unique identifier for each item. 
* Def. Index:
    - Indexes are their namesake. DynamoDB maintains the Index based on the primary-key, 

#### Global Secondary Index 
* Def. Global Secondary Index: 
    - Have partition keys or partition/sort keys that differ from the primary key. 
    - No uniqueness requirment. 

#### Local Secondary Index 
* Def. Local Secondary Index: 
    - Uses the primary-key's partition-key. 
    - Within the objects that have that primary key, you can sort them. 
    - Note. There are storage caveats with LSI's. 

#### Limits
* Note. We are allowed up to five of each index type. 
* Important. Local indexes are only allowed at table creation time. 


### Security and Control

There are two concerns here. (1) *Who* are we giving access to? and (2) *What* are we allowing them to access. AWS defines (1) as a Trust Policy and (2) as an Permissions Policy. DynamoDB has a policy generator on the CLI/Console, or we could just use IAM. 


### Pricing 

To simplify matters, DynamoDB uses its own scale for measuring data transfer. 