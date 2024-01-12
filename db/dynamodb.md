source: [jlafon](https://gist.github.com/jlafon)'s [article](https://gist.github.com/jlafon/d8f91086e3d00c4bff3b)

# An introduction to DynamoDB

DynamoDB is a powerful, fully managed, low latency, NoSQL database service provided by Amazon. DynamoDB allows you to pay for dedicated throughput, with predictable performance for "any level of request traffic". Scalability is handled for you, and data is replicated across multiple availability zones automatically. Amazon handles all of the pain points associated with managing a distributed datastore for you, including replication, load balancing, provisioning, and backups. All that is left is for you to take your data, and its access patterns, and make it work in the denormalized world of NoSQL.

## Modeling your data

The single most important part of using DynamoDB begins before you ever put data into it: designing the table(s) and keys. Keys (Amazon calls them primary keys) can be composed of one attribute, called a hash key, or a compound key called the hash and range key. The key is used to uniquely identify an item in a table. The choice of the primary key is particularly important because of the way that Amazon stores and retrieves the data. Amazon shards (partitions) your data internally, based on this key. When you pay for provisioned throughput, that throughput is divided across those shards. If you create keys based on data with too little entropy, then your key values will be similar. If your key values are too similar, so that they hash to the same shard, then you are limiting your own throughput.

Choosing an appropriate key requires that you structure your DynamoDB table appropriately. A relational database uses a schema that defines the primary key, columns, and indexes. DynamoDB on the other hand, only requires that you define a schema for the keys. The key schema must be defined when you create the table. Individual parts of an item in DynamoDB are called attributes, and those attributes have data types (basic scalar types are Number, String, Binary, and Boolean). When you define a key schema, you specify which attributes to use for a key, and their data types.

DynamoDB supports two types of primary keys, a *Hash Key* and a *Hash and Range Key*.

* A *Hash Key* consists of a single attribute that uniquely identifies an item.
* A *Hash and Range Key* consists of two attributes that together, uniquely identify an item.


### Denormalization

If you've spent some time with relational databases, then you have probably heard of [normalization](http://en.wikipedia.org/wiki/Database_normalization), which is the process of structuring your data to avoid storing information in more than one place. Normalization is accomplished by defining storing data in separate tables and then defining relationships between those tables. Data retrieval is possible because you can join all of that data using the flexibility of a query language (such as SQL).

DynamoDB, being a NoSQL database, and therefore does not support SQL. So instead of normalizing our data, we denormalize it (and eliminate the need to join). A full discussion of denormalizing is beyond the scope of this introductory tutorial, but you can read more about it in DynamoDB's [developer guide](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DataModel.html).

### Accessing data

Performing operations in DynamoDB consumes throughput (which you pay for), and so you should structure your application with that in mind. Individual items in DynamoDB can be retrieved, updated, and deleted. Conditional updates are also supported, which means that the write or update only succeeds if the condition specified is successful. Operations can also be batched for efficiency.

Two other batch operations are also supported, scan and query. A query returns items in a table using a primary key value, and optionally using a range key value or condition. A scan operation examines every item in a table, optionally filtering items before returning them.

### Indexes

Items are accessed using their primary key, but you can also use indexes. Indexes provide an alternative (and performant) way up accessing data. Each index has its own primary key and that key is used when performing index lookups. Tables can have multiple indexes, allowing your application to retrieve table data according to its needs. DynamoDB supports two types of indexes.

* Local secondary index: An index that uses the table's *Hash Key*, but can use an alternate range key. Using these indexes consumes throughput capacity from the table.
* Global secondary index: An index that uses a *Hash and Range Key* that can be different from the table's. These indexes have their own throughput capacity, separate from the table's.

A global secondary indexes is called global because it applies to the entire table, and secondary because the first real index is the primary hash key. In contrast, local secondary indexes are said to be local to a specific hash key. In that case you could have multiple items with the same hash key, but different range keys, and you could access those items using only the hash key.

### Consistency

DynamoDB is a distributed datastore, storing replicas of your data to ensure reliability and durability. Synchronizing those replicas takes time, and may not always be immediately necessary. Because of this, DynamoDB allows the user to specify the desired consistency for reading data. There are two types of consistency available.

* Eventually consistent reads: This is better for read throughput, but you might read stale data.
* Strongly consistent reads: Used when you absolutely need the latest result.

### Limits

DynamoDB is a great service, but it does have limits.

* Individual items cannot exceed 400kb.
* Tables cannot exceed 10Gb.
* If you exceed for provisioned throughput, your requests may be throttled.

## A simple example

To help understand how to use DynamoDB, let's look at an example. Suppose that you wanted to store web session data in DynamoDB. An item in your table might have the following attributes.

| Attribute Name | Data Type |
| ---------------|-----------|
| session_id     | String    |
| user_id        | Number    |
| session_data   | String    |
| last_updated   | String    |
| created        | String    |

In this example, each item consists of a unique session ID, an integer user ID, the content of the session as a String, and timestamps for the creation of the session and the last time it was updated.

The simplest way to access our table is to use *Hash Key*. We can use the `session_id` attribute for the *Hash Key* because it is unique. To look up any session in our session table, we can retrieve it by using the `session_id`.

### Accessing DynamoDB
DynamoDB is provided as an HTTP API. There are multiple libraries that provide a higher level abstraction over the HTTP API, and in many different languages.

[...]
[article](https://gist.github.com/jlafon/d8f91086e3d00c4bff3b) contains a Python with boto example
[...]

# Conclusion
DynamoDB isn't perfect, but it is a great service if you need a scalable, highly available NoSQL database, without having to manage any of it yourself. This tutorial shows how easy it is to get started, but there is much more to it than what is mentioned here.

If you are interested, you should definitely check out these awesome resources.

* [Fine grained access control to DynamoDB](http://docs.amazonaws.cn/en_us/amazondynamodb/latest/developerguide/UsingIAMWithDDB.html)
* [Elastic Map Reduce & DynamoDB](http://aws.amazon.com/blogs/aws/aws-howto-using-amazon-elastic-mapreduce-with-dynamodb/)
* [Data Pipeline & DynamoDB](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-importexport-ddb-part2.html)
* [Redshift & DynamoDB](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/RedshiftforDynamoDB.html)
* [Netflix S3mper filesystem](http://techblog.netflix.com/2014/01/s3mper-consistency-in-cloud.html)