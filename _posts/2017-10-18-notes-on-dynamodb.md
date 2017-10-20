---
title: Notes on DynamoDB
layout: post
date: 2017-10-18
---

## Whitepaper

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

* Def. Scanning: Gets the entire table, and can be filtered. 
* Def: Querying: 
    + Pulls items by their primary keys, or Global/Local indexes. 
    + Know the definitions of the various keys and indexes. 
* Note. Item upload rate is limited at 25/request.
* Note. Filtering: 
    + Dynamo provides normal [expressions](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.OperatorsAndFunctions.html#Expressions.OperatorsAndFunctions.Syntax) for filtering items. 

### Indexes 

* Def. Partition-Key: A chosen attribute for each item whose hash determines it's placing in the database. 
* Def. Sort-Key: A chosen attribute which determines a partial order over the set of Items. 
* Def. Primary-Key: A required unique identifier for each item. Either a primary-key or also composed with a sort-key. 
* Def. Index: A view into the Table using a different primary-key. 
* Def. Global Secondary Index: Have partition keys or partition/sort keys that differ from the primary key. 
    - No uniqueness required. 
* Def. Local Secondary Index: Uses the primary-key's partition-key. 
    - Within the objects that have that primary key, you can sort them. 
* Note. Limits:
    - Local index can only be created along with the table.
    - We are allowed up to five of each index type. 
    - There are storage caveats with LSI's. 


### Security and Control

There are two concerns here. (1) *Who* are we giving access to? and (2) *What* are we allowing them to access. AWS defines (1) as a Trust Policy and (2) as an Permissions Policy. DynamoDB has a policy generator on the CLI/Console, or we could just use IAM. 


### Pricing 

To simplify matters, DynamoDB uses its own scale for measuring data transfer known as Read/Write Capacity Units. 

Let $RPS = \text{reads-per-second}$ and $WPS = \text{writes-per-second}$.
Given an Item in a Table let $|\cdot|$ be the size rounded up to the nearest kilobyte. 

Then,

* Def. Write Capacity Unit:
    - $WPS \cdot |I|$
* Def. Read Capacity Unit: For an item $I$ is 
    -  $RPS \cdot |I|$

Each partition, determined by the partition-key of the primary-key, is given a somewhat equal portion of throughput. 


* Note. Maximum Throughput: Dynamo uses partition-key's to evenly distribute data $\thn$ make sure the keys are uniformly distributed. 


### Reserved Capacity 

Reserved Capacity is a commitment meant to potentially reduce prices in the long run. Can be made in 1 or 3 year spans. 


### Cross Region Replication 

Replication is all good things that come with having multiple backups across the world. It also has the costs associated with such a service. 

### Triggers 

Really just hooks into using Lambda on changes, but limited to js/java/python. Reading from the table is free though. 

### Streams 

Keeps records of changes in a table for the past day. Changes for individual items are in the correct order. No promised order for between items. 

### Time to Live

Lets you set a automatic deletion date on Items. Use cases include stale-data (e.g. logs, sessions) and compliance (ugh). 

* Note. Time Format: Need to use Unix Time. 
* Note. Accuracy: Dynamo runs a cron job to queue up expired items. Deletion has a worst case of two days. 
    - Queries including the item do not treat it specially. 
* Note. Method: Store the time as an attribute in the Items you want to give TTL. 
    - This attribute is not special in any way. 
* Note. Streams: TTL deletions are distinguished form natural ones. 


### DAX

DAX is a fully-managed in-memory cache for DynamoDB. As far as I can tell, it lets you set a up a  cluster when your Region is enough. 


### Virtual Private Cloud Endpoints 

I think it's a barrier between dynamo and the rest of the world. 

###  Storage Backend for Titan

A backend that is specifically for the graph structure. 


### CloudWatch Metrics

It exists. 

### Tagging

This is some mechanism used for cost analysis. 


---

## Developer Guide

### Programming with Dynamo

There are three ways to interface with Dynamo. 

* (1) [Low-Level Interface](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Programming.SDKs.Interfaces.LowLevel.html)
* (2) [Document Interface](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Programming.SDKs.Interfaces.Document.html)
* (3) [High-Level Interface](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Programming.SDKs.Interfaces.Mapper.html)

Each is built on top of the [Low-Level API](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Programming.LowLevelAPI.html). 

* Note. Concerning Completions: We are given the choice between the use Promises in the form of  [`AWSTask`](http://docs.aws.amazon.com/mobile/sdkforios/developerguide/awstask.html#handling-errors), a renamed `BFTask` of Facebooks [Bolts library](https://github.com/BoltsFramework/Bolts-Swift) or standard closure callbacks. 

![Interface Graph](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/images/SDKSupport.SDKInterfaces.png)

#### The Low-Level API

* Headers
    - AWS secures the request by (1) adding a hash of the request in the header and (2) requiring the request reception be within five minutes of departure. Note that we can see useful info in the `X-Amz-...` headers. 
* Body 
    - Uses `json` which is interpreted as attributes. An attribute has a `key`, a `data-type-descriptor`, and `data`. So `a.long.list.of.keys` has a `{ "descriptor" : "data" }` at the end. The exact specifications are [here](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.NamingRulesDataTypes.html?shortFooter=true).
    - Note there are reserved words for keys. 

#### The Low-Level Interface

The section notes that there's a low Low-Level Interface for each SDK. Unfortunately they use Java as their canonical example. The iOS (Swift and Objective-C) one is [here](https://github.com/aws/aws-sdk-ios/tree/master/AWSDynamoDB). 

This is the calling-objective-c-from-swift version. Life would be sad if we didn't have the High-Level Interface. 

* Note. Concerning Optionals: 
    - The example code unwraps only when required.
    - e.g. On `DynamoDB` calls and in dictionaries. See the example below for details. 
    - We follow this convention in all samples. 

```swift
// Note.This code compiles, not sure if it *works*.
// Note. The initialization of `AWSDynamoDBGetItemInput`, `AWSDynamoDBAttributeValue` is one one of those bridging headaches.
let dynamo = AWSDynamoDB.default()

let artist = AWSDynamoDBAttributeValue()
artist?.s = "No One You Know"
let songTitle = AWSDynamoDBAttributeValue()
songTitle?.s = "Call Me Today"
let key = ["Artist": artist!, "SongTitle": songTitle!]

let input = AWSDynamoDBGetItemInput()!
input.tableName = "Music"
input.key = key

dynamo.getItem(input).continueWith { task -> Any? in
    guard task.error == nil else {
        print("Unable to retrive data: ")
        print(task.error!.localizedDescription)
        return nil
    }  
    
    guard let year = task.result?.item?["Year"]?.n else {
        print("No matching song was found")
        return nil
    }
    
    print("The song was released in " + year)
    
    return nil
}

dynamo.getItem(input)
    .continueOnSuccessWith { task -> AWSTask<AWSDynamoDBGetItemOutput> in
        guard let musicItem = task.result?.item else {
            print("No matching song was found")
            return nil
        }
        
        print("The song was released in \(musicItem["Year"]!.n!)")
        return nil
    }.continueWith {task -> Any? in
        guard let error = task.error as NSError? else {
            return nil
        }
        
        print("Unable to retrive data: ")
        print(error.localizedDescription)
        
        return nil
}
```

See this SO Answer by [Sébastien Stormacq](https://stackoverflow.com/a/26964281) to be further convinced of the need for the High-Level Interface.

#### The Document Interface

The document interface is between High-Level and Low-Level Interfaces. Unfortunately, I don't think the iOS SDK has one yet. And they probably won't write one until Swift stabilizes. 


#### The High-Level Interface

See, what they should have done is something like:  

|level|Interface Name|
|:---:|:---:|
| Low | *Something*| 
| Mid | Document | 
| High | Object Persistence | 

Instead they just want to confuse people.

```swift
final class MusicItem: AWSDynamoDBObjectModel, AWSDynamoDBModeling {
    
    var Artist: String?
    var SongTitle: String?
    var AlbumTitle: String?
    var Year: NSNumber?
    
    var somethingInternal: String?
    
    static func dynamoDBTableName() -> String {
        return "Music"
    }
    
    static func hashKeyAttribute() -> String {
        return "Artist"
    }   
    
    static func rangeKeyAttribute() -> String {
        return "SongTitle"
    }
    
    static func ignoreAttributes() -> [String] {
        return ["somethingInternal"]
    }
}


AWSDynamoDBObjectMapper.default().load(MusicItem.self, hashKey: "No One You Know", rangeKey: "Call Me Today")
    .continueOnSuccessWith { task -> AWSTask<AnyObject> in
        guard let musicItem = task.result as? MusicItem else {
            // Note. the block is only run if this task did not produce a cancellation or an error  
            //       => we can assume the item does not exist. 
            print("No matching song was found")
            return nil
        }
        
        print("The song was released in \(musicItem.Year!)")
        
        return nil
    }.continueWith { task -> Any? in
        guard let error = task.error as NSError? else {
            return nil
        }
        
        print("Unable to retrieve data: ")
        print(error.localizedDescription)
        return nil
}
```


#### Handling Errors

Errors objects consist of three components: A code, a name, and a message. 
Errors are marked with a *retry-able* property where not retry-able implies a client side error. 

For Swift, also can handle errors through `AWSTask` or closures. I choose `AWSTask` below. See [the official docs](http://docs.aws.amazon.com/mobile/sdkforios/developerguide/aws-aysnchronous-tasks-for-ios.html#handling-errors-with-awstask) for better details.

The Java example uses the Document Interface. This swift example will use the Low-Level Interface, to the same purpose.


```swift
// Note. This compiles, not sure if it *works*. 
func build<T>(task: AWSTask<T>) {
    task
        .continueOnSuccessWith { task -> Any? in
            guard let result = task.result else {
                print("No result.")
                return nil
            }
            print(result)
            return nil
        }.continueWith { task -> Any? in
            if task.isCancelled {
                print("Canceled.")
            } else if task.isFaulted {
                let error = task.error as NSError?
                print("Recieved the error" + " : " + error.localizedDescription)
            }

            return nil
        }
}

let source = AWSTaskCompletionSource<NSString>()
build(task: source.task)

//source.cancel() // A regular cancelation.
//source.set(result: "Hello world!") // Completed with result.  
//source.set(result: nil) // Completed without result. 
//source.set(error: NSError(domain: "com.ajcaldwell.example", code: 1, userInfo: nil)) // Faulted.
```



## Further Reading 
* [SOSP Paper](http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)
* [Dynamo FAQ](https://aws.amazon.com/dynamodb/faqs/)
* [Dynamo Guide](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)
* [Dynamo Reference](http://docs.aws.amazon.com/amazondynamodb/latest/APIReference/Welcome.html)
* [Dynamo iOS Guide](http://docs.aws.amazon.com/mobile/sdkforios/developerguide/dynamodb-nosql-database-for-ios.html)
