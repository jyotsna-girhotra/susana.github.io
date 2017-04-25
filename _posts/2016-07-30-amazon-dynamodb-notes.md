---
layout: post
title:  "Amazon DynamoDB Notes"
date:   2016-07-30
categories: dynamodb, ansible
---

### Partitions
- 10gb
- a single partition can support max 3000 read capacity units (RCU) , and 1000 write capacity units (WCU)
- (RCU / 3000) + (WCU / 1000) = initial number of partitions (rounded up)
  - e.g. 10000/3000 + 1000/1000 = 5 partitions

### Capacity Units
- capacity units are provisioned on a per table case
- if you use indexes, this increases the read/write capacity units of actions

### Keys
- partition key value is hashed and determines which partition the kv goes into
- if using partition AND sort keys
  - partition key still determines partition
  - partition keys with same value are stored together, and sorted by the sort key (ascending)
  - dynamodb reads in blocks of 4KB so always round up to next 4KB
  - dynamodb writes in 1KB blocks, always round up to next 1KB
- don't rely on burst capacity because it’s using for background maintenance and other tasks without prior notice

### Item Size
- string item is sum of lengths of its attribute names and values using its utf-8 bytes (and utf-8 uses btwn 1-4 bytes..)
- list or map size is (length of the attribute name + sum (length of attribute values) + 3 bytes)
- ASCII character code values are used for string collation and comparison
  - “a” is greater than “A” and “aa” is greater than “B"
- querying multiple items from table must have the same partition key
- can write max of 25 keys at a time
- it’s best if you can fit all data into a single partition (avoiding hot partitions) and make sure that there are no hot keys

### Table Items
- max size of 400KB of the item data in the table PLUS the size of any local secondary index entry (including key vals and projected attribs)
- if you try to read an item that doesn’t exist, dynamo consumes provisioned read throughput
- strongly consistent read req consumes 1 full read cap unit
- eventually consistent read request consumes 0.5 of a read cap unit
- Total Provisioned Throughput / Partitions = Throughput Per Partition
- [You must provide a partition key name and a distinct value to search for.](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/QueryAndScan.html#QueryAndScan.Query)
- ["In a DynamoDB table, the combined partition key value and sort key value for each item must be unique."](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/LSI.html#LSI.Querying)


provisioned capacity units should include those for the table’s local secondary indexes
"For index queries that read attributes that are not projected into the local secondary index, DynamoDB will need to fetch those attributes from the table, in addition to reading the projected attributes from the index."
