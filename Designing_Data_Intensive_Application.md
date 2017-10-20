# Designing Data-Intensive Applications

## Part I. Foundations of Data Systems

### Chapter 1. Reliable, Scalable, and Maintainable Applications
1. Reliability: Tolerating hardware & software faults, human error
2. Scalability: Measuring load & performance, latency, percentils, throughput
3. Maintainability: Operability, simplicity & evolability.

Data-intensive vs compute-intensive.
Raw CPU power is rarely a limiting factor for those applications-bigger problems are usually the amount of data, the complexity of data, and the speed at which it is changing.
A data intensive applications is typically built from standard building blocks that provides commonly needed functionality. e.g.
- Store data so that they, or another applicatoin, can find it again later(databases);
- Remember the result of an expensive operation, to speed up reads(caches);
- Allow users to search data by keyword or filter it in various ways(search indexes);
- Send a message to another process, to be handled asynchronously(stream processing);
- Periodically crunch a large amount of accumulated data(batch processing);

You are not only an application developer, but also a data system designer.
The boundaries between datasystem and message queue are kind of blurred together. Redis can also be sued as messages queue, and message queues with database-like durability guarantees(Apache Kafka). A single tool can not satisfy all the requirements. Application codes hooked all together. e.g. an application-managed caching layer(Memcached), or a full-ext search server(Elasticsearch or Solr) separated from main database.

#### Reliability: Continuing to work correctly, even when things go wrong.
Fault-tolerant: it only makes sense to talk about tolerating certain types of faults.
Fault is defined as one component of the system deviating from its spec, whereas a failure is when the system as a whole stops providing the required service to the user.
NetFlix Chaos Monkey induces faults to ensure teh fault-tolerance machinery is continually exercised and tested.
Tolerate faults that can be cured.

Hardware Faults: multiple-machine redundancy was only required by a small number of applications for which high availability was absolutely essential. Hence there is a move toward systems that can tolerate the loss of entire machines, by using software fault-tolerance techniques in preference or in addition to hardware redundancy. Such sysetmc can be without downtime of the entire system(a rolling update)

Software Errors: a systematic error within the system. Hard to participate, and correlated nodes, and they tend to cause many more system failures than uncorrelated hardware faults.
It is making assumption about its environment. There is no quick solution to the prolbme of systematic faults in software. Small thins can help: thinking about assumptions and interactions in the system; thorough testing; processing isolation; allowing process to crash and restart; measuring, monitoring and analyzing system behavior in production.

Human errors: minimize opportunities for errors; decouple the places where people make the most mistakes from the places where they can cause failures; test thoroughly at all levels; allow quick and easy recovery from human errors; set up detailed and clear monitoring; implement good management practices and training.

#### Scalability: describe a system's ability to cope with increased load.
Twitter:
1. Posting a tweet simply inserts a new tweet into a global collection of tweets. When a user requests their home timeline, look up all the people they follow, find all the tweets for each of those users, and merge them(sorted by time).
    Select tweets.*, users.* From tweets
        Join users on tweets.sender_id = users.id
        Join follows On follows.followee_id = users.id
        Where follows.follower_id = current_id
2. Maintain a cache for each user's home timeline. When a user posts a tweet, look up all the people who follow that user, and insert the new tweet into each of their home timeline caches. The requests to read the home timeline is then cheap, because its result has been computed ahead of time. Because the average rate of published tweets is almost two orders of magnitued lower than the rate of home timeline reads, and so in this case it's preferable to do more work at write time and less at read time. (Hybrid) But for celebrities who has lots of followers, each write will result in millions of writes to home timeline! So tweets from any celebrities that a user may follow are fetches separately and merge with that users's home timeline when it is read.

Load parameters depends on the architecture of your system: request per second, the ratio of reads to writes in a database, the number of simultanesouly active users in a chat room, the hit rate on a cache.

Performance:
1. when you increase a load parameter and keep the system resources unchanged, how is the performance of your system affected?
2. When you increase a load parameter, how much do you need to increase the resources if you want to keep performance unchanged?

Batch processing system such as Hadoop, care about throughput: the number of records we can process per second, or the total time it takes to run a job on a dataset of a certain size. 
Online system, Response time: the time between a client sending a request and receiving a response.

Latency vs response time:
resonset timeis what the client sees: time to process the requests + network delays + queuing delays. Latency is the duration that a request is waiting to be handled-during which it is latent, awaiting service.

A distribution of values instead of a single number. May intrinsically more expensive: process more data. Or some variation: context switch to a background process, the loss of a network packets and TCP retransmission, a garbage collection pause, a page fault forcing a read from disk, mechnical vibrations in the server rack, and etc.

Median: 50 percent percentile, or p50.
High percentiles of response time: tail latencies.Optimizing the 99.99th percentile was deemed too expensive.
The belowing contracts define the expected performance and availability of a service.
SLO: service level objectives
SLA: service level agreements.

Queuing delays often account for a large part of the response time at high percentiles. When generating load artifically in order to test the scalability of a system, the load-generating client needs to keep sending requests independently of the repsonse time.

Scaling up: vertical scaling
Scaling out: horizontal scaling, distributing the load across multiple machines, or shared-nothing architecture.

While distributing stateless services across multiple machines is fairly straightforward, taking stateful data systems from a single node to a ditributed setup can introduce a lot of complexity. Common wisdom until recently was to keep your database on a single node(scale up) until scaing cost or high-availability requirements forced you to make it distributed.

There is no magic scaling sauce that can fit all.

#### Maintainability
The majority of cost of software is not in its initial development, but in its ongoging maintenance.
- Operability: making life easy for operations
- Simplicity: Managing Complexity. Remove accidental complexity: if it is not inherent in the problem that the software solves bur arises only from implementation. One of the best tools we have for removing accidental complexity is abstraction.
- Evolvability: Making change easy.


### Chapter 2. Data Models and Query Languages
Most applications are built by layering one data model on top of another. For each layer, the key question is: how is it represented in terms of next-lower layer? Each layer hides the complexity of the layers below it by providing a clean data model.

1. As an application developer, you look at the real world and model it in terms of objets or data strucures, and APIs that manipulate those data structures. Those structures are often specific to your application.
2. When you want to store those data structures, you express them in terms of a general purpose data model, such as JSON or XML documents, tables in a relational database, or graph model. data storage and querying.
3. The engineers who built your database software decide on a way of representing that JSON/XML/relational/graph data in terms of bytes in memory, on disk, or on a network. Storage engine and how data models are actually implemented.

#### Relational Model versus Document Model
Relational Model, i.e., SQL: data is organized into relations(called tables in SQL), where each relation is an unordered collectionof tuples(rows in SQL).
Network model and the hierarchical model came and went.
In 2010s, NoSQL(Not Only SQL). several driving force for NOSQL: a need for greater scalability, including very large datasets or very write throughput; a preference for open and free source software over commercial database products; specialized query operations not supported by relational model; restrictiveness with relational schemas, and a desire for a expressive data model.

The object-relational mismatch: impedance mismatch between object and relation
Object-relational mapping(ORM) frameworks like Active Record and Hibernate.
3 ways to apply one-to-many relationship in SQL:
1. the most common normalized representation is to put them in seperate tables, with a foreign key reference to the use table;
2. LAter version of the SQL standard support for structured data and XML data, allowing multi-value data to be stored within a single row. Also JSON.
3. encode them as a JSON or XML documents, store it on a text column in the database. You cannot use the database to query for value inside that encoded column.

Document-oriented databse: MongoDB, RethinkDB, CouchDB, Espresso. For a data structure like a self-contained document, JSON representation can be quite appropriate.
The Json representation has better locality than multi-table schema, and all the relevant informationis in one place, and one query is sufficient. Json makes one-to-many tree strucutre explicit.

Many-to-one and Many-to-Many relationships
Using id over string:
- Consistent style and spelling across profiles
- Avoiding ambiguity
- Ease of updating
- Localization support
- Better search
The advantage of using an ID over string is that it has no meaning to humans, and it never needs to change. Removing such duplcation is the key idea behind normalizatoin in database.

Normalizing *many-to-one* relationships doesnot fit into the document model. In document databases, joins are not needed for *one-to-many* tree strutures, and support for joins is often weak. The work of making hte join is shifted from the database to the application code. Moreover, data has a tendency of becoming more interconnected as features are added to applications.

Hierarchical model
Network model: access path
Relational model: the query optimizer does al the hard work for you, and will automatically use whichever indexes are most appropriate.
Document databases reverted back to the hiearchical model in one aspect: storing nested records within their parent record rather than in a seperate table.
When it comes to representing many-toone and many-tomany relationships, relational and document databases are not fundamanetally different: in both cases, the related item it referenced by a unique identifier, which is called `foreign key` in the relational model and a `document reference` in the document model. It is resolved at read time by using a join or a follow-up queries.

Pros:
    1. document data model: schema flexibility, better performance due to locality, cose to the data structure used by application;
    2. better suppport for join, many-to-one and many-to-many relationships

Cons:
    1. document data model: cannot directly refer to a nested item within a document, but as long as it is not too deeply nested, it is not a problem; poor join, but if application, for example, records which events occurred at which time, which doesn't need many-to-many relationships. if using many-to-many relationships, reduce the need for joins by denormalizing, but then the application code needs to do additional work to keep the denormalized data consistent. Or Joins can be simulated by making multiple requests to the databases, but that move complexity into the applcation and is usually slower than a join performed by speciliazed code inside the database.

For highly-interconnnected data, the document data is awkward, the relational model is acceptable, and graph models are the most natural.

Schema-on-read: document database. The structure of the data is implicit, and only interpreted when the data is read. It is similiar to dynamic (runtime) type checking.
Schema-on-write: relational database. The schema is explicit,and is enforced. Static (compile) type checking.

If changing the format of its data, in document model, you write new documents with new filed when reading old documents, while in relational model, you migrate and update. Schema changes have a bad reputation of being slow and requring downtime. But Most relationship database systems execute `ALTER TABLE` in a few ms, while MySQL takes mean minutes or even hours of downtime because it copies the entire table. Running the `UPDATE` statement on a large table is likely to be slow on any database.

Schema war:
The schema-on-read approach is advantageous if the items in the collections don't all have the same strucutre for some reason(heterogenous). But in cases all records are expected to have the same structures, schemas are a useful mechnism for documenting and enforcing that structure.

Locality:
A document is usually stored as a single continuous string, encode as JSON, XML, or a binary variant. If your application often need to access the entire document, there is performance advantage to this storage locality. The locality advantage only applies if you need large parts of the document at the same time. On updates to a document, the entire document ususally needs to be rewritten-only modifications that don't change the encoded size of a document can easily be performed in place. It is recommened that you keep documents fairly small and avoid writes that increase the size of a document. 
Locality is not limited to document model, for example, Google's Spanner, Oracle's multi-table index cluster table, and column-family in the Bigtable model(in Cassandra and HBase)

Covergence of document and relational databases

### Quering language for data
SQL: declarative language. You specify the pattern of the data you want but not how to achieve that goal. It is up to the database system's query optimizer to decide which indexes and which join methods to use, and in which order oto execute various parts of the query.
declarative languages often lend themselves to parallel execution.
In a web browser, using delcarative CSS stylingis much better than manipulating styles imperatively in JavaScript.

MapReduce Querying is not neither a declarative query language nor a fully imperative query API, but somewhere between: the logic of the query is expressed with snippet of code, which are called repeatly by the processing framework.

### Graph-like Data Model: for many-to-many relationships
A graph consists of two kinds of objects: vertices(or nodes or entities) and edges(or relationships or arcs), e.g, Socical, web, road or rail. Vertices can be homogeneous or heterogeneous.
- Property graphs: Cyper Query language created for Neo4j graph database.
- Triple-store model: SPARQL. DataLog

More data model:
1. Genome database
2. Large scale data analysis working with hundreds of petabytes
3. Full-text search is arguably a kind of data model that is frequently used alongside databases.


## Storage and Retrieval
two storage engines:
1. log-structured storage engines
2. page-oriented storage engines such as B-trees.

Appending to a file is generally very efficient. Similiarly to that, many databases internally use a `log`, which is an append-only data file.
Note: `log` is often used to refer to application logs, where an application outputs text that describes what's happening, but in database it means "an append-only sequence of recores". It doesn't have to be human-readable; it might be binary and intended only for other programs to read.

O(n) is not efficient in database.
In order to efficiently find the value for a particular key in the database, we need a different data structure: an index. The general idea behind them is to keep some additional metadata on the side, which acts as a signpost and helps you to locate the data you want.
An index is an `additional` structure that is derived from the primary data. It doesn't affect the contents of the database; it only affects the performance of queries. Maintaining addtional structures incurs overhead, especially on writes, because the index also needs to be updated every time data is written.
Tradeoff: well-chosen indexes spped up read queries, but every index slows down writes.

### Hash Indexes
Indexes for key-value data, a useful building block for more complex indexes. Key-value stores are quite similiar to the `dictionary` type.
Index data on disk: keep an in-memory hash map where every key is mapped to a byte offset in the data file. Whenever you append a new key-value pair to the file, you also update the hash map to reflect the offset of the data you jut wrotes. This is the approach Bitcask uses. If that part of the data file is already in the filesystem cache, a read doesn't require any disk I/O at all.
A storage engine like this is well suited to situtations where the values for each key is update frequently.
To avoid running out of disk space, break the log into segments of a certain size by closing a segment file when it reaches a cerain size, and making subsequent writes to a new segment file. then perform `compaction` on these segments, which throws duplicate keys in the logs, and keeps only the most recent update for each key. Then merge. Compaction and merging can be done in a background thread, and while it is going on, we can still continue to server read and write requests as normal, using the old segment files. After the merging is done, we sweitch read requests to using the new merged segment instead of the old ones- and then the old segment files can simply be deleted. Each segment now has its own in-memory hash table, mapping keys to file offsets. In order to find the value for a key, we search backforwards.
Some issues in a real implementation:
1. file format: instead of CSV, use a binary format that first encodes the length of a string in bytes, followsed by the raw string.
2. delete records: append a special delete record to the data files(called a tombstone). When merged, the tombstones tells the mreging process to discard any previous values for the deleted key.
3. Crash recovery: if crash happens, restore hash map by reading the segment again. Sppeds up by storing a snapshot of each segment's hash map on disk.
4. Partially wirtten records: include checksums, detecting corrupted parts of the logs.
5. Concurrency control: have only one writer thread, and be read concurrently by multiple threads.

Appending-only logs's pros:
1. it is sequential write operations, faster than random writes.
2. concurrency and crash recovery are much simpler.
3. merging old segments avoid the data fragmentation over time.
Hash table index's cons:
1. must fit in memory, so you can't have a very large number of keys.
2. Range queries are not efficient.

### SSTables and LSM-Trees
Sorted String Table: the sequence of key-value pair is sorted by key and no duplicated key in each table.
Advantages:
1. Mergeing segments is simple and efficient, as they are sorted by keys, using mergingSort.
2. no longer need to keep an index of all keys in memory. Just need an in-memory index to tell you the offsets for some of the keys, but it can be sparse.
3. It is possible togroup a range of records into a block and compress it before writing it to disk. It can save disk space and reduce I/O bandwidth use.

#### Constructing and Maintaining SStables
Maintaining a sorted structure on disk is possible(B Trees), but maintaining it in memory is much easier(AVL or red-black trees).
Make our storage engine work as follows:
1. when a write comes in, add it to an in-memory balanced tree, called a `memtable`.
2. When the memtable gets bigger than some threshold - write it out to disk as an SSTable file. The new SSTable file becomes the most recent segment of the database. While the SSTable is being written, writes can continue to a new memtable instance.
3. To serve a read, first try to find the key in the memtable, then in the most recent on-disk segment, then in next-older one, etc.
4. from time to time, merge and compact segments.
5. in case of crash, keep a seperate log on disk to which every write is immediately appended, which is not in sorted order.

Making an LSM-Tree out of SSTables.
The above algorithms is used in LevelDB and RocksDB, key-value storage engine libraries. Similiar storage engines are used in Cassandra and HBase, inspired by Google's Bigtable paper.
Log-Structured Merge-Tree(LSM-Tree)
Storage engines that are based on this principle of merging and compacting sorted files are called LSM storage engines.
Lucene, an indexing engine for full-text search used by Elasticsearch and Solor, use a simliar method for its term dictionary, where key is a word and the value is the list of IDs of all the documents that contain the word.
The basic idea of LSM-tree-keeping a cascade of SSTables that are merged in the background- is simple and effective. Since data is sorted in order, range query is efficient and because the disk write are sequential the LSM-Tree can support high write throughput.

### B-Trees.
The log-structured indexes are not the most type of index. B-tree is the most commmon one.
Like SSTables, B-trees keep key-value pairs sorted by key, which allows efficient key-value lookups and ranges queries. While log-structured indexes break the database down into variable-size segments and always write sequentially, B-trees break the database into fixed-sized blocks or pages, traditional 4KB, and read or write one page at a time. This design corresponds more closely to the underlying hardware.
Each page can be identified using an address or location, similar to a pointer, but on disk instead of in memory. Following the references, eventually we get down to a leaf page, which either contains the value for each key inline or contains references to the pages where the values can be found.
Branching factor: the number of references to child pages in one page of B-tree, typically serveral hundred.
B-trees is balanced, which mean a B-Tree with n keys always has a depth of O(lgn) and requires O(lgn) page read from disk.
In order to make the database resilient to crashes, it is common for B-tree implementations to include an additional data structure on disk: a write-ahead log(WAL, or redo log), which is an append-only log.
Cocurrency control is complicated than log-structured approaches, and need to use latches(lightweight locks).
Optimization: copy-on-write; abbreviate the key; leaf page locality; Addtional pointers, for example, referring to its sibling pages to the left and right; fractal trees.

### B-Trees vs LSM-Trees
As a rule of thumb, LSM-Trees are typically faster for writes, whereas B-trees are thought to be faster for reads.

Updating a B-trees in place. Inserting a record could split the page and write two pages that were split, and also overwrite their parent page to update the parent page.

Advantages of LSM-Trees:
LSM-Tree has lower write amplification and reduced fragmentation, and sequential writes.
Downsides of LSM-Trees:
compactions process can sometimes interfere with the performance of ongoing reads and writes. B-trees: each key exists exactly one place in the index, which is good for transaction and locks can be directly attached to the tree.

### Other indexing structures
A primary key uniquely identifies one row in a relational table, or one document in a document database, or one vertex in a graph database.
A secondary index can be constructed from a key-value index, and it doesn't need to be unique.

You can store the actual value or a reference to the value stored elsewhere which is called heap file efficient for updating.
If the extra hop from the index to the heap file is too expensive, store the indexed row drectly within an index. This is known as a `clustered index`, used in MySQL's InnoDB storage engine.

Multi-column indexes: the most common type of multi-column index is called a `concatenated index`, combining several fields into one key by appending one column to another, e.g. `first_name last_name`.

Full-text search and fuzzy indexes

### Keeping everything in memory
In-memory key-value stores, such as Memcached, are intending for caching use only, where it's acceptable for data to be lost if a machine is restarted. RAMCloud is an open source, in-memory key-value sotre with durability. Redis and Couchbase provide weak durability by writing to disk asynchronously.
Besides performance, in-memory database is providing data models that rea difficult to implement with disk-based indexes. REdis offers a database-like interface to various data structures such as priority queues and sets.

### transaction processing or analytics?
Transaction: a group of reads and writes that form a logical unit.
Transaction doesn't necessarily have ACID. Transaction processing just means allowing clients to make low-latency reads and writes; bach processing only run periodically.
Access pattern for interactive application: online transaction processing(OLTP).
business analysis pattern: online analytic processing(OLAP).
database for OLAP is called `data warehouse`.

#### Data Warehousing
The process of getting data into the warehouse is known as `Extract-Transform-Load` (ETL).

#### The divergence between OLTP databases and data warehouse
The data model of a data warehouse is most commonly relational.
Data warehouse vendors such as Teradata, Vertica, SAP HANA, and ParAccel sell their systems under expensive commercial license. Amazon Redshift is a hosted version of parAccel. Open source SQL-on-Hadoop projects: Apache Hive, Spark SQL, Cloudera Impala, Facebook Presto, Apache Tajo, and Apach Drill. Some of them are base on ideas from google's Dremel.

Stars and snowflakes: schemas for analytics
At the center of the schemas is a `fact table`. Each row of the fact table represents an event that occurred at a particular time. Some of the columns in the fact table are attributes, while others are foreign key references to other tables, called dimension tables.
A variation of this template is caled `snowflake schema`, where dimensions are further broken into subdimensions.

###### Column-Oriented Storage
In most OLTP databases, storage is laid out in a row-oriented fashion.
Column-Oriented storage store all the values form each column together.

Column compression: use bimap along with run-length encoding.
Caculating the bitwise OR of several bitmaps is very efficient, because each bit in one column's bitmap corresponds to the same row as the bit in another column's bitmap.

Memory bandwidth and vectorized processing

Sort order in column storage
Writing to column-oriented storage: using LSM-trees.

Aggregation: Data cubes and materialized views, for performance boost.
A materialized view is an actual copy of he query results, written to disk, whereas a virtual view is just a shortcut for writing queries.

### Summary:
Two categories:
1. OLTP: user-facing. Disk seek time is often the bottleneck.
    - log-structured school, which permits appending to files and deleting obsolete files, but never update a file that has been written. Bitcask, SSTables, LSM-Trees, LevelDB, Cassandra, HBase, Lucene
    - update-in-place school, which treats the disk as a set of fixed-size pages that can be overwritten. B-trees are the biggest example of this philosophy, being used in all major relational databases and also many nonrelational ones.
2. OLAP: business analysts. Disk bandwidth(not seek time) is the bottleneck here, and column-oriented storage is an increasingly popular solution.

log-structured storage engines are a comparatively recent development. Their key idea is that they systematically turn random-access writes into sequantial writes on disk, which enables high throughput due to the performance characteristics of hard drives and SSDs.



## Encoding and Evolution
Relational database generally assumes that all data in the database conforms to one schema. Bye constrast, schema-on-read databases don't enforce a schema, so the database can contain a mixture of older and newer data formats written at different times.
Rolling update: deploy the new version to a few nodes at a time.
Backward and forward compatibility. Forward compatibility is harder to achieve.

Formats for encoding data:
The translation from the in-memory representation to a byte sequence is called encoding(also known as serialization or marshalling), and the reverse is called decoding(parsing, deseriliazation, unmarshalling).

Built-in support in languages: Java has java.io.Serializable, Ruby has Marshal, Python has pickle, and third-party libraries, such as Kryo for Java.
Problems within language-specific formats:
1. bound to a specific language
2. security reson. need to instantiate arbitrary class.
3. versioning data.
4. Efficiency.

Json, xml and binary variants
JSON, XML and CSV are textual formats, and thus somewhat human-readable, but:
1. encoding of numbers. In XML and CSV, you cannot distinguish between a number and a string that happens to consist of digits. In JSON, you cann't distinguish integers and floating-point number, and it doesn't specify a precision.
2. JSON and XML have good support for Unicode character string, but they don't have that for binary strings. Peopel get around this limitation by encoding the binary data as text using Base64. But the schema must be sued to indicate the value should be interpreted as Base64-encoded, and it increases the data size by 33%.
3. Optional schema support for XML/JSON, but complicated. applications that don't use XML/JSON schemas need to potentially hardcode the appropriate encoding/decoding logic instead.
4. CSV doesn't have schema and also is a quite vague format.
As long as people agree on what the format is, it often doesn't matter how pretty or efficient the format is.


Binar encoding
JSON is less verbose then XML, but both still use a lot of space compared to binary formats.
Binary encoding for JSON: MessagePack, BSON, BJSON, etc.

Thrift and Protocol Buffers are binary encoding libaries. Both require a schema for any data that is encoded.

Field Tags and schema evolution: you can only add or remove a field that is optional.

Datatypes and schema evolution

Avro: the writer's schema and the reader's schema. You many only add or remove a field that has a default value.

Code generation and dynamically typed language.
Thrift and Protocol Buffers rely on code gneration: after a schema has been defined, you can generate code that implements this schema in a programming language of your choice.

Binary encoding based on schemas:
1. much compact
2. the schema is a valuable form of documentation.
3. Keeping a database of schemas allow you to check forward and backforward compatibility.
4. For users of statically type programming languages,the ability to generate code from schema is useful, since it enables type checking at compile time.


### Modes of Data Flow
How data flows between process
#### Dataflow through Databases
Data outlives code.
Rewriting(migrating) data into a new schema is certainly possible, but it's an expensive thing to do on a large dataset, so most database avoid it if possible.
#### Dataflow through services: REST and RPC
service-oriented architecture(SOA)/microservice architecture.
web services:
    1. REST. A design philosophy on top of HTTP.
    2. SOAP. It is using an XML-based language called Web Service Description language, useful in statically programming languages, but less so in dynamically typed ones. SOAP is too complex to construct manually, falling out of popularity.
A definition format such as OpenAPI, known as Swagger, can be used to describe RESTful APIs and produce documentation.

#### the problems with RPC
RPCs: Enterprise JavaBeans(EJB), Java's Remote Method Invacation(RMI), Common Object Request Broker Architecture(CORBA)
They are fundamentally flawed. A network request is very different from a local function call.
REST seems to be the predominant style for public API.The main focus of RPC frameworks i son requests between services owned by the same organization, typically within the same datacenter.

Evolution for Restful api: use a version number in the URL or in the Http Accept header.

#### Message-passing Dataflow
Asynchronous message-passing systems, somehwere between RPC and databases. They are similiar to RPC in that a client's request is delivered to another process with low latency. They are similiar to databse in that message is not sent via a direct network connection, but goes via an intermediary called a `message broker`(message queue or message-oriented middleware), which store the message temporarily.
Advantages over RPC:
1. act as a buffer
2. automatically redeliver messages.
3. avoid the sender needing to know the recipient.
4. allow one message to be sent to several recipients.
5. decouple sender from recipient.
But it is usually one-way: a sender normally doesn't expect to receive a reply to its message. the communication pattern is asynchronous: the sender doesn't wait for the message to be delivered, but simply send it and then forgets about it.

Message Brokers: RabbitMQ, ActiveMQ, HornetQ, NATS, Apach Kafka.

There could be many producers and consumers on the same queue.. Message broker typically doesn't enforce any particular data model- a message is just a sequence of bytes with some metadata, so you can use any encoding format.

#### Distributed Actor Frameworks:
Akka, Orlean, Erlang OTP.


## Distributed Data
shared-nothing == horizontal == scaling out
Replication: keep a copy of the same data on several different nodes, providing redundancy.
Partitioning: split a big database into smaller subsets called partitions so that different partitions can be assigned to different nodes(also know as sharding).

### Chapter 5. Replication
Why replicate data:
1. keep data geographically close to your users
2. to allow the system to continue working if some of its parts have failed. Increase availability
3. scale out the number of machines that can server read request to increase read throughput.
All the difficulty in replication lies in changes to replication data.

Three algorithms for replcating changes between nodes: single-leader, multi-leader, and leaderless replication.

#### Leaders and Followers
Replica: each node that stores a copy of the database is called replica.
Every write to the database needs to be processed by every replica. The most common solution for this is called leader-based replication(or active/passive or master/slave replication).
It works as follows:
1. One of the replicas is designeated the leader(master or primary). It accept write request from clients and first writes the new data to its local storage.
2. The other replicas are known as followers(read replicas, slave, secondaries, or hot standby). whenever the leader wirtes new data to its local storage, it also sends the data change to all of its followers as part of a replication log or change stream. Each follower takes it and apply the updates.
3. a client can read from either the leader or any of the followers. Writes are only accepted on the leader.
This mode is used in relational database, such as PostgreSQL, MySQL, Oracle, and nonrelational database, including MongoDB, RethinkDB, and ESpresso.

Synchronous vs Asnchronous replication
In relational database, synchronouse or asynchronous is often a configurable option; other system are often hardcoded to be either one or the other.
The advantage of synchronous replcation is that the follower is guaranteed to have an up-to-date copy that is consistent with the leader. The disadvatage is that if the synchronous follower doesn't respond, the write cannot be processed and the leader must block all writes and wait until the synchronous replica is available again.

Semi-synchronous: having one of the followers to be synchronous and others asynchronous.

Often, leader-based replication is configured to be completely asynchronous. In this case, if the leader fails and is not recoverable, any writes that have not yet been replicated to followers are lost. This means that a write is not guaranteed to be durable, even if it has been confirmed to the client. But the advantage is that the leader can continue processing writes, even if all of its followers have fallend behind.

##### setting up new follower
1. take a consistent snapshot of the leader's dataase at some point in time.
2. copy the snapshot to the new follower node.
3. The follower connects to the leader and requests all the data changes that have happened since the snapshot was taken. This requries that the snapshot is associated with an exact position in the leader's replication lo: in PorstgreSQL the log sequence number, in MySQL binlog coordinates.
4. When the follower has processed the backlog of data changes since the snapshot, it has caught up. It can now continue to process data changes from the leader as they happen.

##### Handling Node Outages
- Follower failure: catch up recovery
    Each follower keeps a log of the data changs it has reeived from the leader. If crashing and restarting, or network outage, the follower can recover easily: from its log, it know the last transaction, thus it request all the lastest data changes during the disconnection.
- Leader Failure: failover
    1. Determing taht the leader has failed.
    2. Chooseing a new leader.
    3. Reconfiguring the system to use the new leader.
    Problems:
        1. Lost writes on new leader or conflicting writes if a new leader comes back.
        2. Inconsistency between different storage systems
        3. two leaders: split brain
        4. choose a right timeout.

##### Implementation of replication logs
1. statement-based replication: VoltDB, MySQL before 5.1
    In the siplest case, the leader logs every write request and sends it to its followers.However, it has issue with nondeterministic function, such as NOW(), or RAND(), and autoincrementing column, and statements that have side effects.
2. Write-ahead Log (WAL) shipping: PostgreSQL, and Oracle
    Every write is appended to a log either in case of a log-structured storage engine, or a B-tree. The main disadvantage is that the log describe the data on a very low level: a WAL contains details of which bytes were changed in which disk block. If the database changes its storage format form one to naother version, it is not compatible, and thus upgradeds require downtime.
3. Logical(Row-based) log replication: MySQL's binlog
    Use different log formats for replication and for the storage engines. this kind of replication log is called a logical log, which is a sequence of records describing writes to database tables at the granularity of a row. It is easier for external application to parse.
4. Trigger-based replication: Databus for Oracle, Bucardo for Postgres
    A trigger lets you register custom application code that is automatically executed when a data change occurs in a database. You can log this change to a seperate table, ready for an external process.

#### Problems with Replication lag
In read-scaling architecture, you can increase the capacity for serving read-only request simpley by adding more followers, but it only works iwth asynchronous replcation, and there is a prolbem with replication lag, even though it has `eventual consistency`.

1. Reading your own writes
read-after-write consistency or read-your-write consistency. Other user's updates may not be visible until some later time.

implementation: 
    1. always read the user's own profile from the leader, and any other users' profiles from a follower. When reading something that the user may have modified, read it from thr leader, otherwise, read it from a follower.
    2. if most things are potentially editable, use other criteria: track the time of the last update, and for one minute after the update, make all reads from the leader; or track the replication lag.
    3. The client can remember the timestamp of its most recent write, the system can ensure that the replica serving any reads for that user reflects updates at least until that timestamp. The timestamp could be a logical timestamp or a real system clock.
    4. Additional complication when distributed across multiple datacenters.
Additional issues:
    corss-device read-after-write. The metadata like timestamp will need to be centralized, and alll the request should be routed to the same datacenter.

2. Monotonic Reads
Another anomaly is moving backward in time if a user's requests go to different replicas each time.
Monotonic reads is a guarantte that this kind of anomaly does not happen. It's a less guarantee than strong consistence, but a stronger guarantee than eventual consistency.
One way of achieving that is to make sure that each user alway makes their reads from the same replica. The replica can be chosen based on a hash of the user ID, rather than randomly.

3. Consistent Prefix Reads
This gurantee says that if a sequence of writes happens in a certain order, then anyone reading those wries willl see them appear in the same order. This is a particular problem in partitioned(sharded) databases.
One solution is to make sure that any writes that are causually related to each other are written to the same partition.

#### Solutions for Replication lag
When working with an eventually consistent system, it is worth thinking about how the application behaves if the repliation lag increase to several minutes or even hours. If the anser is "no problem", it's greate.
The reason for transaction: they are a way for a database to provide stronger gurantee so that the appliation can be simpler.

#### Multi-leader replication(mster-master or active/active replication)
Allow more than one node to accept writes. Eanch node that processes a write must forward that data change to all other nodes. Each leader simultaneously acts as a follower to the other leaders.

Use cases for multi-leader replication:
In a multi-leader configuration, you have a leader in each datacenter.

Compare single-leader and multi-leader configuration fare in a multi-datacenter deployment:
- performance: in multi-leader datacenter, every write can be processed in the local datacenter and is replicated asynchronously to the other datacenters.
- Tolerance of datacenter outages: in multi-leader, each datacenter can operate independently and replication catches up when the failed datacenter comes back online.
- Tolerance of network problems: inter-datacenter link does not prevent writes being processed.

database support with external tools: Tungsten Replicator for MySQL, BDR for PostgreSQL

Downsides with multi-leader: write conflicts due to concurrent writes.
Due to subtle configuration pitfalls and surprising interactions with other database features, multi-leader replication is often considered dangerous.

#### Clients with offline operation, like calendar app on multiple devices. 
Each app has a local database that acts as a leader. CouchDB is designed for this mode of operation.

#### Collaborative editing
real-time collaborative editing appliation allow several people to edit a document simultanesouly, like Google Docs.
If you want to guarantee that there will be no editing conflicts, the application must obtain a lock on the document before a user can edit it. This collaboration model is equivalent to single-leader replication with transactions on the leader.
For faster collaboration, you may wan to make the unit of change very small and avoid locking. It allows multiple users to edit simultanesouly, but it brings all the challenges of multi-leader replication, including requiring conflict resolution.

The biggest problem with multi-leader replication is that write conflicts can occur, which means that conflict resolution is required. If two users change both place to different values.

Synchronous versus asynchronous conflict detection
In a multi-leader setup., both write are succesful and the conflicts is only dected asynchronously at some later pointer in time. At the time, it may be too late to ask the user to resolve the conflict.

Solution to conflicts:
1. Conflict avoidance: The simplest approach.
    For example, in an application where a user can edit their own data, you can ensure the requests from a particular user are always routed to the same datacenter and use the leader in that datacenter for reading and writing.
2. Converging toward a consistent state
    A single leader database applies writes in a sequential order, but in a multi-leader configuration, there is no defined ordering of writes.
    Given each write a unique ID(e.g., a timestamp, a long random number, a UUID, or a hash of the key and value), pick the writer with the highest ID as the winner, and throw away the other writes. LWW: last writer wins. This will lead to data loss.
    Or Given each replica a unique ID, and the higher number wins; It will lead to data loss too.
    Or merge the value together;
    Or record the conflict in an explicit data strucutre that preserver all information, and write application that resolves the conflict at some later time. As the most appropriate way of resolving a conflct may depend on the application, most multi-leader replcation tools let you write conflict logic using appliation code. The code may be executed on write or on read. Bucardo allows you to write a snippet of Perl for on write. CouchDB return multiple versions of the data to the application on read.
Automatic conflict resolution:
1. Conflict-free replicated datatypes
2. Mergeable persistent data structure
3. Operation tranformation, the conflict resolution algorithm behind collaborative editing application such as Google Docs.

#### Multi-leader replication topologies
1. Circular: MySQL. single-point of failure
2. Start: single-point of failure
3. All-to-all: due to network congestion, it will lead to a problem of causality, that is "consistent prefix reads".
Conflict detection are poorly implemented in may multi-leader replication systems.

#### Leaderless Replication
The client directly sends its writes to several replicas, or a coordiante node does this on behalf of the client. Unlike a leader databse, that coordinate does not enforce a particular ordering of writes. Amazon's Dynamo system is such kind, and Riak, Cassandra, and Voldemort.
Read request are also sent to several nodes in parallel, in case that some nodes go down. The client may get diferent responses from different nodes, thus version number is used to determine the latest one.

To solve the problem of unavailable nodes coming back:
1. Read repair. The read client writes the newer value back.
2. Anti-entropy process

Quorums for reading and writing
In Dynamo-style databases, teh parameter n(node), w(writes), r(reads) are configurable. A common choice is to make n an odd number and to set w=r=(n+1)/2. if w+r > n, at least one of the r replicas you read from must have seen the most recent successful write.
Dynoma-style databases are generally optimized for use cases that can tolerate eventual consistency.

Monitoring staleness.

Sloppy Quorums: Writes and reads still require w and r succesfful response, but those may includes nodes that are not among the designated n"home" node for a value.
Hinted handoff: once thenetwork interrruption is fixed, any writes that one node temporarily accepted on behalf of another node are sent to the appropriate "home" nodes.

Leaderless replication is also suitable for multi-database operation, since it is designed to tolerate conflicting concurrent writes, network interruptions, and latency spikes.

Last write wins(Discarding concurrent writes): it achieves the goal of eventual convergence, but at the cost of durability.

A operation A happens before B if B knows about A, or depends on A, or builds upon A in some way. Whether one operation happens before another operation is the key to defining what concurrency means. For defining concurrency, exact time doesn't matter.

#### Capturing the happens-before relationship
The server maintains a version number of every key, increments the version number every time that key is written,and stores the new version number along with the value written. When a client writes a key, it must include teh version nmber from the prior read, andit msut merge together all values that it received in the prior read. when a client reads a key, the server returns all values that have not been overwritten,as well as the latest version number.

Merging concurrently written values: to allow people to also remove things from their carts, and not just add things, then taking the union of siblings may not right. In such case, you need a deletion marker: tombstone.

#### Summary
Purpose of replications:
1. High availability: keep the system running even when one machine goes down.
2. Disconnected operation: allow an application to continue working when there is a network interruption.
3. Latency: Placing data geographically close to users, so that users can interact with it faster.
4. Scalability: Being able to handle a higher volumes of reads than a single machine could handle, by performing reads on replicas.

Single-leader replication is popular because it is fairly easy to understand and there is no conflict resolution to worry about. Multi-leader and leaderless replication can be more robuts in teh presence of faulty nodes, network interruptions, and latency spikes- at the cost of behing harder to reason about and providing very weak consistency.

### Chapter 6. Partitioning
Break the data up into partitions, also known as sharding.
partition: shard in MongoDB, Elasticsearch, and SolrCloud; region in HBase; tablet in Bigtable, vnode in Cassandra and Riak; vBucket in CouchBase.

Partitions are defined in such a way that each piece of data(each record, row, or document) belongs to exactly one partition. Each partition is a small database of its own, although database may support operations that touch multiple partitions at the same time.

The main reason for wanting to partition is scalability. A large dataset can be distributed across many disks, and the query load can be distributed across many processors.

Partitioned databases were pioneered in the 1980s by products such as Teradata and Tandem NonStop SQL, and more recently rediscovered by NoSQL databases and Hadoop-based data warehouses.

#### Partitioning and replication
A node may store more than one partition. Each node may be the leader for some partitions and a follower for other partitions.
The choice of partitioning scheme is most independently of the choice of replication scheme.

##### Partitioning of key-value Data
If the partitioning is unfair, we call it skewed. A partition with disproportionately high load is called a hot spot.

1. partitioning by key range:A-B, C-E
    This partitioning strategy is used by Bigtable, HBase, RethinkDB, and MongoDB before v2.4. To avoid hotspots in teh sensor databse, prefix each timestamp with the sensor name.

2. partitioning by hash of key
    use a good hash function and take skewed data and make it uniformly distributed. But we lose a nice property of key-ranging partitioning. MongoDB send any range query to all partitions. Range queries on the primary key are not supported by Riak, Counchbase, or Voldemort. Cassandra achieves a compromise by using a compound primary key conisting of several columns, and the first key is hashed to find the partition, but the others are sued as a concatenated index for sorting the data in SSTable.

##### Skewed workloads and relieving hot spots
On a social site, a celebrity user with millions of followers may result in a large volum of writes to the same key.
It is the responsiblity of the application to reduce the skew. For example, add additional random number to key allows those keys to be distributed to different partitions. But they have to read the data from all different keys and combine it. It also requires additional bookkeeping.

#### Partitioning and Secondary Indexes
A secondary index usually doesn't identify a record uniquely but rather is a way of searching for occurrences of a particular value: find all cars whose color is red.
1. partioning secondary index by document: local index
Each parition is completely seperate: each partitioin maintains its own secondary index, covering only that documents in that partition. This approach is called scatter/gather. It makes read queries on secondary indexes quite expensive. widely used: MongoDB, Riak, Cassandra, ElasticSearch, SolorCloud, and VoltDB.
2. partitioning secondary indexes by term: global index. Amazon's DynamoDB.
    Construct a global index taht covers data in all partitions, and it must alos be partitioned. It makes reads more efficient, but writes more expensive.

#### Rebalancing Partitions
The process of moving load from one node in the cluster to another is called rebalancing.
Minimum requirements:
1. fairly distributed load
2. no downtime.
3. No more data than necessary should be moved

Strategies for rebalancing:
Don't hash mode N: when the number of nodes changes, it will frequently move data.
1. Fixed Number of partitions: create more partitions thatn there are nodes, and assign serveral partitions to each node. But choosing the right number of partitions is difficult if the total size of the dataset is high variable.
2. Dynamic paritioning: split and merge.
3. Partitioning proportionately to nodes.

Automatic vs manual rebalancing
It can be good to have a human in the loop for the rebalancing. It's slower than automatic, but helps prevent operational surprises.

#### Request Routing
Service discovery: many companies have written their own in-house service discovery tools.
The knowledge of which partition is assigned to which node:
1. nodes. Allow cients to contain any node (via a round-robin load balancer).
2. Routing tier, which determines the node that should handle the request and forwards it accordingly.
3. client. clients can connect directly to the node.

Many distributed data system rely on a seperate coordination service such as ZooKeeper to keep track of the cluster metadata. Other actors can subscribe to this information in Zookeeper.
Cassandra and Riak take a different approach: a gossip protocol among the nodes to disseminate any chanes in cluster state, which is the first approach.

#### Parallel Query Execution
Massively parallel processing relational database. the MPP query optimizer breaks this complex query into a number of execution stages and partitions, many of which can be executed in parallel on different nodes of the database cluster.
Simple queries that read or write a single key is about the level of access supported by most NoSQL distributed datastores.


### Chapter 7. Transations
The slippery concept of transaction:
The safety guarantee provided by transcations are often described by ACID: atomicity, consistency, isolation, and durability. ACID is a very vague term.

Atomicity: in the context of ACID, atomicity is not about concurrency. The ability to abort a transaction on error and have all writes from that transaction discarded is `atomicity`. Perhaps abortability is a better term.

Consistency: dpends on teh application's notion of invariants, and it's the application's responsibility to define its transaction correctly so that they preserve consistency.

Isolation: concurrenly executing transactions are isolated from each other: they cannot step on each other's toe. The result is the same as if they had run serially, but in reality they may have run concurrently.

Durablity: the promise that once a transaction has committed successfully, any data it has written will not be forgotten, even if there is a hardware fault or the database crashes. There is no perfect durability.

Many nonrelational database don't have such a way of grouping operatin together, while in relational databases, everything between BEGIN TRANSACTION and a COMMIT is considered to be part of the same transaction base on TCP connection.

Single-object operations is universally guaranteed to be atomic, but they are not transactions. A transaction is a mechanism for grouping multiple operations on multiple objects into one unit of execution.

Although retrying an aborted transaction is a simple and effective error handling mechanism, it isn't perfect.


#### Weak isolation levels
Serializable isolation has a performance cost, and many databases don't want to pay that rice. So many databases use weak levels of isolation, which protect against some concurrency issues, but not all.
- Read Committed: no dirtry reads and no dirty writes.
  Read Committed does not prevent the race condition between two counter increments.
  Implementation: use write locks and remember both the old committed value and the new value set by transaction that currently hold the write lock.
  It is the default setting in Oracle 11g, PostgreSQL, SQL Server 2012, and etc.

- Snapshot isolation: to prevent time anomaly, called read skew or nonrepeatable read.
  It is useful for backups and analytic queries and integrity checks.
  MVCC: multi-version concurrency control

- Preventing lost update.
  1. Atomic write operations:
    May databases provide atomic update operations, which remove the need to implement read-modify-write cycle in application code.
    e.g., UPDATE counters SET value = value + 1 WHERE key = 'foo';
    Document databases such as MongoDB provide atomic operations for making local modifications to a part of JSON document, and REdis provides atomic operations for modifying data structures such as priority queue. 
    Atomic operations are implemented by taking an exclusive lock on the object when itis read so that so other transaction can read it until the update has been applied.It is known as cursor stability. Another option is to simpley force all atomic operations to be executed on a single thread.
    Object-relational mapping frameworks make it easy to write unsafe read-modify-write cycles instead of using atomic operations provided by the database.
  2. Explicit locking
    BEGIN transaction;
    select * from foo;
    for update // indicate this should be locked
    update xxxxx
    commit;
  3. Automatic detecting lost updates: if it detects a lost updates, the transaction manager abort it and force it to retry its cycle. It is supported by PostgreSQL, Oracle, but not by MySQL/InnoDB.
  4. Compare-and-set
    In database that don't provide transaction, you sometimes find an atomic compare-and-set operation. It is to avoid lost updates by allowing an update to happen only if teh value has not changed since you last read it.
    e.g. two user modify the wiki concurrently
    UPDATE wiki_page SET content = 'new content'
      WHERE id = 1234 AND content = 'old content';
    But if the database allows the WHERE clause to read from an old snapshot, this statement may not prevent lost updates, because the condition may be true even though another cocurrent write is occurring.
  5. Confflict resolution and replication
    Locks and comapre-and-set operations assume that there is a single up-to-date copy of the data, thus technique based on locks or compare-and-set do not apply in this context.
    In repliated database, a common approach is to alow concurrent writes to create several conflicting versions of a value(sibling), and to use application code or special data structures to resolve and mrege these versions after thet fact.
    Atomic operation can work well in a replicated context, especially if they are commutative, as in Riak.
    LWW, last write wins, is prone to lost updates, which is the default in may replicated databases.

- Write skew and Phantoms
Two kinds of race conditions, dirty writes and lost updates, should be prevented either automatically by the databse, or by manual safeguard such as using locks or atomic wrie operations. Dirty write will mix up writes from different transactions. Lost update happens in read-modify-write cycle.
More race conditions:
    doctor oncall system, two doctors want to be off oncall at the same time. Since they are updatting two different object, it is neither a dirty write nor a lost update.
    Write skew happens when two transactions read the same objects, and then update some of those objects.
    Atomic single-object operations and the automatic detetion of updattes can not prevent write skew. Some databases allow you to configure constraints, but you would need a constraint that involves multiple objects. Most databases do not have built-in support for such constraints, but you may be able to implement them with triggers or materialized views.
    If you can't use a serialized isolation level, the second-best option in this case is probably to expicitly lock the rows that transaction depends on.
    More write skew examples: Meeting room booking system, multiplayer game, claiming a username, preventing double-spending. Those check for the absence of rows matching some serach condition, and the write adds a row matching the same condition. If the query in step 1 doesn't return any rows, SELECT FOR UPDATE can't attach locks to anything.
    One solution is to materialize conflicts to turn a phantom into a lock conflict. But it is hard and error prone to let a concurrency control mechinism leak into the application data model.
    A serializable isolation level is much preferable in most cases.

#### Serializability
Serializable isoatation is usually regarded as the strongest isolation level. It guarantees that even though transactions may execute in parallel, the end result is the same as if they has executed one at a time, serially, without any concurrency.

***Actual Serial Execution***
Simplest: to execute only one transaction at a time, in serial order, on a single thread.
If multi-threaded concurrency was considered essential for getting good performance during the prevoius 30 years, two developments causes those changes: 1. Cheaper RAM; 2. OLTP only make short and small number of reads and writes, while long-running analytic queries, mostly read-only, can be run on a consistenct snapshot outside of the serial execution loop.
It is implemented by VoldDB/H-Store, Redis, and Datomic. It can perform bettern than multi-threaded counterpart, as it avoids the overhead of locking. However, its throughput is limited to that of a single CPU core.

***Encapsualting transactions in stored procedures***
Systems with single-thread serial transaction processing don't allow interactive multi-statement transactions, which send back and forth queries and results between appliation code and the database server, causing much delay. Instead, the application must submit the entire transaction code to the database ahead of time, as a stored procedure.

***Pros and Cons of stored procedures***
Stored procedurs have existed for some time in relational database, and they have been part of the SQL standard(SQL/PSM) since 1999. 
Cons: each database vendor has it own language for stored procedure, but they look quite ugly and archaic; code that running in a database is difficult to manage; a database is often much more performance-sensitive than an application server.
Pros: Modern implementations of stored procedures have used existing general-purpose programming languages instead: Redis use Lua, VoltDB use Java or Groovy, Datomic uses Java or Clojure.
VoltDB also uses stored procedures for replication.

***Partitioning***
For this single-threaded database to scale to multiple CPU cores, and multiple nodes, you can potentially partition your data, which is supported in VoltDB. Each partition can have its own transaction processing thread running independently from the others. But cross-partition trasactions have addtional coordination overhead.

***Summary of serial execution***
Constaints: 1. every transaction must be small and fast; limted to use cases where the active dataset can fit in memory; write throughput must be low enough; cross-partition transactions should be restrained.

##### Two-Phase Locking(2PL)
Two-phase locking, one widely used algorithms for serializability in database, makes the lock requirement strongre. Several transactions are allowed to concurrently read the same object as long as nobody is writing to it. But as soon as anyone wants to write an object, exculsive access is required:
1. If transaction A has read an objet, B cannot writes to that object;
2. If A has written an object, B cannot read that object. Reading an old version of the object, is not acceptable under 2PL.
Snapshot isolation has the mantra `readers never block writers, and writers never block readers`, which capture the key difference between snapshot.
2PL protects against all race conditions.

***Implementation of 2PL***
Thise first phase(whiel the transaction is executing) is when the locks are required, and the second phase(at the end of transactions) is when all the locks are released. The lock can either be in shared mode or exclusive mode. The database detects deadlocks between transactions.

The big downside of 2PL is performance due to reduced concurrency mostly and also locking overheads.

***Predicate Locks***
Predicate locks works similarly to the shared/exclusive lock, but rather than belonging to a particular object, it belongs to all objects that match some search conditions.
Or index-ranged locks because predicate locks do not perform well.

##### Promising: SSI, serializable snapshot isolation(SSI), which provides full serializablity but only has a small performace penalty.
  Compared to 2PL, the big advantage of serializable snapshot isolation is that one transaction doesn't need to block waiting for locks held by another transaction. Loke wunder snapshot isolation, writers don't block readers, and vice versa.
Implemented by PostgreSQL in single-node mode and by FoundationDB in distributed mode.
  Compared to serilal execution, it is not limted to the throughput of a single CPU core.
The rate of aborts significantly affects the overall performance of SSI.

Pessimestic versus optimisti concurrency control
Pessimestic: serial excution and 2PL
Optimistic: SSI

##### Decision based on an outdated premise
1. Detecting reads of a stale MVCC object version(uncommitted write occurred before the read)
2. Detecting writes that affect prior reads.

####Summary
Transactions are an abstract layer that allows an application to pretend that certain concurrency problems and certain kinds of hardware and software faults don't exist.
- Dirty reads: one client reads another client's writes before they have been committed. The read committed isolation level and stronger levels prevent dirty reads.
- Dirty writes: one client overwrites data that another client has written, but not yet committed. Almost all transaction implementations prevent dirty writes.
- Read skew(nonrepeatable reads): A client sees different parts of the database at different points in time. This issue is most commonly prevented with snapshot isolation, which allows a transaction to read from a consistent snapshot at one point in time. It is usually implemented with multi-version concurrency control.
- Lost updates: Two clients concurrency perform a read-modify-write cycle. One overwrites the other's write without incorpoarting its changes, so data is lost. Some implementations of snapshot isolation prevent this anomaly automatically, while others require a manual lock(SELECT FOR UPDATE).
- Write skew: a transaction reads something, makes a decision based on the value it saw, and writes the decision to the database. However, by the time the writes is made, the premise of the decision is no longer true. Only serilazable isolation prevents this anomaly.
- Phantom reads: a transaction reads objects that match some search condition. Another client makes a write affects the result of that search. Snapshot isolation prevents straightforward phantom reads, but phantoms in the context of write skew requires special treament, such as index-ranged locks.

Weak isolation levels protect against some of the anomalies but leave you, the application developer, to handle other manually. Only serializable isolation protects all of these issues. 
- Literally exectuting transaction in a serial order
  If you can make each transaction very fast to execute, and the transaction throughput is low engough to process on a single CPU core, this is a simple and effective option.
- Two-phase locking
  For decades, this has been the standard way of implementing serializability, but many application avoid using it because of its performance characteristics.
- Serializable snapshot isolation(SSI)
  A fairly new algorithm that avoids most of the downsides of the previous approaches. It uses an optimistic approach, allowing transactions to proceed without blocking. Whena transaction wants to commit, and it is aborted if the execution was not serializable.



### Chapter 8. The Trouble With Distributed Systems
Working with distributed systems is fundamentally different from writing software on a single computer.

***Faults and partial failures***
The operation on a single compute is always `deterministic`: either is works or it doesn't.
In a distributed system, there may well be some parts of the system that are broken in some unpredictable way, even though some other parts of the system are working fine. This is known as `partial failure`, which is `nondeterministic`.
This nondeterminism and possibility of partial failures is what makes distributed systems hard to work with.

***Cloud computing and Supercomputing***
Large-scale computing systems:
- high-performance computing: for computationally intensive scientific computing tasks, such as weather forcasting or molecular dynamics.
- cloud computing: multi-tenant datacenters. Commodity computers connected with an IP network, elastic resource allocation, and metered billing.

If we want to make distributed systems work, we must accept the possibility of partial failure and build fault-tolerance mechanisms into the software. In other words, we need to build a reliable system from unreliable components.

***Unreliable Networks***
***Detecting Faults***
***Timeouts and Unbounded delays***
***Network congestions and queuing***
Rather than using configured constant timeouts, system can continually measure response times and their variability(jitter), and automatically adjust timeouts according to the observed response time distribution. This can be done with a Phi Accrual failure detector, which is used in Akka and Cassandra.

***Synchronous vs Asynchonous networks***
Bounded delay in telephony networks, using dedicated circuits.
Datacenter networks and the internet are optimized for bursty traffic, not suitable for packet switching. 
ATM: a hybrid networks that support both circuit switching and packet switching.
With Qos and admisson control,it is possible to simulate circuit switching on packet networks.

***Unreliable clocks***
Monotonic vs time-of-day clocks
Clock synchronization and accuracy
Relying on Synchrnoized clocks: if you use software that requires synchronized clocks, it is essential tht you alos carefully monitor the clock offsets between all the machines. Such monitoring ensures that you notice the broken clocks before they can cause too much damage.

***Timestamps for ordering events***
LLW(last write wins) is widely used in both multi-leader replication and leaderless databases such as Cassandra and Riak. Some implementations generate timestamps on the client rathern than the server, bu tthis doesn't change the fundamental problem with LLW: database writes can disappear; LWW cannot distinguish between writes sequential and concurrent; two nodes generate writes with the same timestamp. 
NTP's synchronization accuracy is limited by the network round-trip time, in addition to other sources of error such as quartz drift.

Logical clocks: base on incrementing counters rather than an oscillating quartz crystal, a safer alternative for ordering events.

***Clock readings have a confidence interval***
[earliest, latest]

Synchronized clocks for global snapshots
If two intervals do not overlap, we are sure the causality between them. Google Spanner deploys a GPS receiver or atomic clock in each datacenter, allowing clocks to be synchronized to within 7 ms.

#####Process Pauses
A node in a distributed system must assume that its execution can be paused for a significant length of time at any point, even in the middle of a function. During the pause, the rest of the world keeps moving and may even declare the paused node dead because it's not responding.
e.g.: garbage collector, virtual machine suspension, context switch, page fault and thrashing, disk I/O, system signal;

***Response time guarantee***
Hard real-time systems: must meet the deadline, such as aircraft, rocket, robots, cars.
Providing real-time guarantees in a system requires support from all levels of the software stack and is quite expensive.

***Limiting the impact of garbage collection***
1. treat GC pauses like brief planned outages of a node.
2. or use the GC only for short-lived objects and to restart processes periodically.

####Knowledge, Truth, and Lies

##### The truth is defined by the majority
Many distributed algorithms rely on a quorum: decisions require some minimum number of votes from several nodes in order to reduce dependence on any one particular node.

***The leader and the lock***
only one of some thing, e.g.:
1. one leader for a database partition
2. only one transaction for clients to hold the lock
3. only one user to register a particular username.

Use a fencing token: checking a token on the server side and rejecting older token.

A system is Byzantine fault-tolerant if it continues to operate correctly even if some of the nodes are malfunctioning and not obeying the protocol, or if malicous attackers are interfering with the network. Protocols for making systems Byzantine fault-tolerant are quite complicated, and fault-tolerant embedded system rely on support from the hardware level.

#### System Model and Reality
System model: an abstraction that describes what things an algorithm may assume
1. synchronous model: not a realistic model of most practical systems
2. partially synchronous model: realistic model of many systems
3. asynchronous model

Node failures:
1. Crash-stop
2. Crash-recovery
3. Byzatine faults

The partial synchronous model with crash-recovery faults is the most useful one.

Correctness of algorithms:
Safety property is defined as `nothing bad happens`; liveness as`something good eventually happens`.

Theoretical analysis and empirical testing are equally important.

####Summary
Reason to use a distributed system: scalability, fault-tolerant, and low latency.


### Chapter 9. Consistency and Consensus

Consistency Guarantees
Most replicated databases provide at least eventual consistency, which means if you stop writing to the database and wait for some unspecified length of time, then eventually all read requests will return the same value. Or we call it `convergence`.

A database looks superfically like a variable that you can read and write, but i fact it has much more complicatd sematics.
The edge cases of eventual consistency only become apparent when there is a fault in the system or at high concurrency.

Transaction isolation is primarily about avoiding race conditions due to concurrently executing transactions, whereas distributed consistency is mostly about coordinating the state of replicas in the face of delays and faults.

####Linearizablity
also known as atomic consistency, strong consistency, or immediate consistency, external conistency.
Linearizablity is a `recency guarantee`.

#####What makes a system linearizable?
To make a system appear as if there is only a single copy of the data.

register: it could be a key in a key-value store, one row in a relational database, or one document in a document database.

It is possible to test whether a system's behaviro is linearizable by recording the timings of all requests and response, and checking whether they can be arranged intoa valid sequential order.

A database may provide both serializability and linearizablity, and this combination is know as strict serializability or strong one-copy serializability. Implementations of serializability based on 2PL or actual serial execution are typically linearizable. However, serializable snapshot isolation is not linearizable.

Locking and leader election
Coordination services like Apache ZooKeeper and etcd are often used to implement distributed locks and leader election. Libraries like Apache Curator help yb providing higer-level recipes on top of ZooKeepter. A linearizable storage service is the basic foundation for these coordination tasks.

Constraints and uniqueness guarantees require linearizability, such as username, bank balance, seat occupancy.

Cross-channel timing dependencies will result in inconsistency.

Replication method:
1. Singler-leader replication(potentially linearizability)
2. Consensus algorithms(linearizable)
3. Multi-leader replication(not linearizable)
4. Leaderless replication(probably not linearizable)

#####The cost of linearizability
A network interrruption forces a choice between linearizability and availability

The CAP Theorem
A better way of phrasing CAP would be either Consistent or Available when Partitioned.
CAP only considers one consistency model(namely linearizability) and one kind of fault(network partitions). It doesn't say anything about network delays, dead nodes, or other trade-offs. Thus, although CAP has been historically influential,it has littel practical value for designing systems.

Many distributed database choose not to provide linearizable guarantee primarily to increase performance, not so much for fault tolerance.

If your application requires linearizability, and some replicas are disconnected from the other replicas are disconnected from the other replicas due to a network problem, then some replicas cannot process requests while they are disconnected: they must either until the network problem is fixed, or return an error.

If your application does not require linearizablity, then it can be written in a way that each replicas can process requests independently, even if it is disconnect from other replicas. In this case, this application can remain available in the face of network problem, but its behavior is not linearizable.

####Ordering Guarantees
There are deep connections between ordering, linearizability, and consensus.

Ordering and causality
If a system obeys the ordering imposed by causality, we say that it is `causally consistent`.

The causal order is not a total order
A `total order` allows any two elements to b compared, so if you have two elements, you can always say which one is greater and which one is smaller.
for example, mathematical sets are not totally ordered: is {a,b} greater than {b,c}? Mathematical sets are partially ordered.

Different database consistency models:
- Linearizability: total order. No concurrency.
- Causality: two operations are concurrent if neither happened before the other. Causality defines a partial order.

Linearizablity is stronger than causal consistency
A system can be causally consistent without incurring the performance hit of making it linearizable(in particular, the CAP theorem does not apply). In fact, causal consistency is the strongest possible consistency model that does not slow down due to network delays, and remains available in the face of network failures. Causal consistency is quite recent, and not much of it has yet made its way into production systems.

Capturing causal dependencies
Sequence number ordering: create sequence numbers in a total order that is consistent with causality. Such a total order captures all the causality information, but also imposes more ordering thatn strictly required by causality. In a single-leader database, the leader can simply increment a counter for each operation.

Noncausal sequence number generator: 
In multi-leader or leaderless database, use even/odd sequence numbers or attach timestamps or preallocate bolcks of sequence numbers.

#####Lamport timestamps
The lamport timestamp is simply a pair of (counter, node ID). Two nodes may sometimes have the same counter value, but by including the node ID in the timestamp, each teimstamp is made unique. Every node and client keeps track of the maximum counter value it has seen so far, and includes the maximum on every request. When a node receives a request or response with a maximum counter value greater than its own counter value, it immediately increase its counter to that maximum. But ordering ---> increase of lamport timestamp, while increase of lamport timestamp cannot -----> ordering.

Lamport timestamp is not sufficient.

####Total Order Broadcast
Total order broadcast requires two safety properties:
1. Reliable delivery:
   No messages are lost: if a message is delivered to one node, it is delivered to all nodes.
2. Totally ordered delivery
  Messages are delivered to every node in the same order.

Using Total order broadcast
Consensus service such as ZooKeeper and etcd actually implement total order broadcast.

State machine replication for each database replcation.

Implementing linearizable storage using toal order broadcast
Total order broadcast is asynchronous: messages are guaranteed to be delivered in a fixed order, but there is no guarantee about when a message can be delivered(replication lag). By contrast, linearizablity is a recency guarantee: a read is guaranteed to see the latest value written.

####Distributed Transactions and Consensus
Goal: get several nodes to agree on something.
Situation in which it is important to agree on:
1. leader election: no split brain in a single-leader database.
2. Atomic commit: in the event that a transaction may fail on some nodes but succeed on others, we have to get all nodes to agree on the outsome of the transaction: either they all abort/roll back or they all commit.

FLP result: there is no algorithm that is always able to reach consensus if there is a risk that a node may crash. The predicate is that a determinstic algorithm cannot use any clocks or timeout in the asynchronous system model.
However, distributed systems can usually achieve consensus in practice.
    
#####Atomic comit and two-phase commit(2PC)
Atomicity prevents failed transactions from littering the database with half-finished results and half-updated objects.
On a single node, write data and commit. It is a single node that makes the commit atomic.
As for mulitple nodes involved in a transaction, most "NoSQL" distributed stores do not support such distributed transactions, but various clustered relational systems do.

A transaction commit must be irrevocable, which forms the basis of `read committed`.
2PC uses a coordinator, which is often implemented as a library or a seperate process or service, such as Java EE container, Narayan, JOTM, BTM, or MSDTC. The coordinator sends a 'prepare' request to all participants, waits for acks, then send a `commit` request, then waits for acks.
1. when the application wants to begin a transaction, it requests a transaction ID from the coordinator.
2. The application begins a single-node transaction on each of the participants and attaches the globally unique xid. if anythiing goes wrong in this stage, the coordinator or any of the participants can abort.
3. Once the application is read to commit, the coordinator send a prepare request to all participants. If any filas or times out, the coordinator sends an abort request for that xid to all participants.
4. When a participants receives the prepare request, it makes sure that it can definitely commit the transaction under all circumstances. The participant surrenders the right to abort the transaction, but without actually committing it.
5. When the coordinator has recieves responses to all prepare requests, it makes a denitive decision. This is called the commit point.
6. Once the decision has ben written to disk, the commit or abort request is sent to all participants. If this request fails or timeout, the coordinator must retry forever until it succeeds.

Coordinator failure: the commit point of 2PC comes down to a regular single-node atomic commit on the coordinator. The coordinator must writes its commit or abort decision to a transaction log on disk before sending it to participants.

***In practice***, distributed transactions provides an important safety guarantee, but are criticized for causing operational problems, killing performance. Many cloud services hoose not to implement it.

Two different types of distributed transactions:
1. Database-internal distributed transactions: MySQL
2. Heterogeneous distributed transactions

Exactly-once message processing requires all systems affected by the transaction are able to use the same atomic commit protocol. e.g. message broker + atomic distributed transaction

XA transactions: eXtended Architecure is a standard for implementing two-phase commit across heterogeneous technologies.

Holding locks in doubt: database transaction usually take a row-level exclusive lock on any rows they modify. In serializable isolation, a database using 2PL also have to take a shared lock on any rows read by the transaction.

Recovering from coordinator failure
Orphaned in-doubt transaction will block for ever, so it needs manual intervention.
Many XA implementation have an emergency escape hatch called heuristic decision for probaly breaking atomicity.

####Fault-tolerant consensus
A consensu algorithm must satisfy the following properties:
1. Uniform agreement: No two nodes decide differently.
2. Integrity: No node decides twice.
3. validity: if a node decides value, then v was proposed by some node.
4. termination: every node that does not crash eventually decides some value.RES

The best-known fault-tolerant consensus algorithm are Viewstamped Replication(VSR), Paxos, Raft, and Zab. Most of these algorithms decide on a sequence of values.

Epoch numbering and quorums
for the uniqueness of the leader, all of the consensus protocls make a weaker guarantee: they define an epoch number and guarantee that within each epoch, the leader is unique. Each election is given an incremented epoch number. Two rounds of voting: once to choose a leader, and a second time to vote on a leader's proposal.

Limitation of consensus:
1. weaker performance
2. require a strict majority to operate
3. static membership
4. in case of highly variable network delays, frequent leader elections.

####Membership and Coordination services
ZooKeeper or etcd are often described as "distributed key-value stores" or "coordination and configuration services". HBase, Hadoop YARN, OpenStack Nova, and Kafaka all rely on ZooKeeper running in the background.
ZooKeeper is modeled after Google's Chubby lock service.
ZooKeeper and etcd are designed to hold small amounts of data that can fit entirely in memory. That small amount of data is replicated across all the nodes using a fault-tolerant total order broadcast algorithm.
Additional features of ZooKeeper:
1. Linearizble atomic operations: a distributed lock is implemented as a `lease`.
2. Total ordering of operations
3. Failure detection: heartbeats between clients and servers.
4. Change notifications

Assigning partitioned resource to nodes can be achieved in ZooKeeper. Apache Curator provides higher-level tools on top of ZooKeeper.
The kind of data managed by ZooKeeper is quite slow-changing. If appliation state needs to be replicated from one node to another, other tools (such as Apache BookKeeper) can be used.

Service discovery
ZooKeeper, etcd, and Consul are also often used for `service discovery` to find out which IP addresses you need to connect to in order to reach a particular service.

Membership services: it determines which nodes are currently active and live member of a cluster.

####Summary
Linearizability: makes a database behave like a variable in a single-threaded program, although being slow. All operations in a single, totally ordered timeline.
Causality: weaker consistency, some things can be concurrent.

Consensus equivlance:
1. linearizable compare-and-set registers
2. atomic transaction commit
3. total order broadcast
4. locks and lease
5. membership/coordination service
6. Uniqueness constraint

If the single leader fails,
1. wait for it to recover
2. manually choose a new leader;
3. use an algorithm to automatically choose a new one.

## Part 3. Derived Data
On a high level, systems that store and process data can be grouped into two broad categories:
1. Systems of record, or source of truth:
   It holds the authoritative version of your data, like user input. each fact is represented exactly once and normalized.
2. Derived data systems
  Data ins a derived system is the result of taking some existing data from another system and transforming or processing it in some way, such as cache, summary data.
Derived data is redundant and denormalized.
The distinction between system of record and derived data systm depends not on the tool, but on how you use it in your application.

###Chapter 10. Batching Processing
Three types of systems:
1. Services(online systems)
  A service waits for a request from a client to arrive and handle it ASAP. Metrics: response time and availability.
2. Batch processing systems(offline systems)
  It takes a large amount of input data, runs a job to process it, and produces some output data. Metrics: throughput
3. Stream processing system(near-real-time systems)
Some where between online and offline/batch processing.

MapReduce, a batch processing algorithm published in 2004, was implemented in Hadoop, CouchDB, and MongoDB.

####Batch processing with unix tools
Mergesort has sequential access pattern that perform well on disks. The sort utility in GNU automatically handles large-than-memory datasets by spilling to disk, and automatically parallelize sorting across multiple CPU cores. This means that the simple chain of Unix commands scale to large datasets, without running out of memory. The bottleneck is the disk read rate.

The Unix philosophy:
1. Make each program do one thing well. To do a new job, build afresh rather than complicate old programs by adding new "features".
2. Expect the output of every program to become the input to another, as yet unknown, program. Don't clutter output with extraneous information. Avoid stringently columnar or binary input formats. Don't insist on interactive input.
3. Design and build software, even operating systems, to be tried early, ideally within weeks. Don't hesitate to throw away the clumsy parts and rebuild them.
4. Use tools in preference to unskilled help to lighten a programming task, even if you have to detour to build the tools and expect to throw some of them out after you've finished using them.
Automation, rapid prototying, incremental iteration, being friendly to experimentation, breaking down large projects into manageable chunks.

***A uniform interface***
In Unix, that interface is a file, an ordered sequence of bytes.

***Seperation of logic and wiring***
Seperating the input/output wiring from the program logic makes it easier to compose small tools into bigger systems.

***Transparency and experimentation***
Unix makes it easy to see what is going on:
1. the input files to Unix commands are treated as immutable.
2. You can end the pipeline at any point.
3. You can write the output of one pipeline stage to a file and use that file as input to the next stage. This allows you to restart the later stage without rerunning the entire pipeline.

####MapReduce and Distributed Filesystems
The biggest limitation of Unix tools is that they run only on a single machine-and that's where tools like Hadoop come in.

MapReduce is a bit like Unix tools, but distributed across potentially thousands of machines.
MapReduce jobs read and write files on a distributed filesystem. In Hadoop's implementation of MapReduce, that filesystem is called HDFS, an opensource reimplementation of the Google File System(GFS).
Various distributed file systems: HDFS, GlusterFS, Quantcast File system, Amazon S3, Azure Blob Storage, OpenStack Swift.
HDFS is based on `shared-nothing`, in contrast to the shared-disk approach of Network Attached Storage(NAS) and Storage Area Network(SAN) architectures. Shared-disk stoage is implemented by a centralized storage application, often using custom hardware and special network infrastructure such as Fibre Channel. The share-nothing approach requries no special hardware, only computers connected by a conventional datacenter network.

HDFS consists of a daemon process running on each machine, exposing a network service that allow other nodes to access files stored on that machine. A central server called the `NameNode` keeps track of which file blocks are stored on which machine. File blocks are replicated on multiple machines. No special hardware in HDFS.

MapReduce Job Execution
1. read a set of input files, and break it up into records;
2. call mapper function to extract a key and value form each iput record; awk
3. sort all of the key-value pairs by key. sort
4. call the reducer function to iterate over the sorted key-value pairs. uniq
map and reduce are where you write you own data processing code.

distributed execution of mapreduce
The mapper and reducer only operate on one record at a time; the framework can handle the complexities of moving data between machines.
In Hadoop MapReduce, the mapper and reducer are each a Java class that implements a particular interface. In MongoDB and CouchDB, mappers and reducers are javascript functions.

The input to a job is typically a directory in HDFS, and each file or file block within the input directory is a seperate partition that can be processed by a separate map task.
The MapReducer scheduler tries to put the compuation near the data, that is to run each mapper on machines taht has a replicas of the input file.
Each map task partitions its output by reducer, based on the hash of the key.
The process of partitioning by reducer, sorting, and copying data partitions from mappers to reducers is known as shuffle.
The reducer task takes the files from the mappers and merges them together, preserving the sort sort.

***MapReduce workflow***
Chain mapreduce jobs into workflow.
To handle thses dependencies between job executions, various workflow scheduler for Hadoop have been developed, including Oozie, Azkaban, Luigi, Airflow, and Pinball.
Higher-level tools for Hadoop, such as Pig, Hive, Cascading, Crunch, and FlumeJava set up workflows of multiple MapReduce stages.

***Reduce-Side Joins and Grouping***
To analyze the clickstream data, take a copy of the user database and put it in the same distributed filesystem as the log of user activity events. Have the user databse in one sef of files and the user activity records in another set of files.

Sort-Merge Joins
One set of mappers would go over the activity events(extracting the user ID as the key and the activity event as the value), while another set of mapper would go over teh user database(extracting the user ID as the key and the user's date of birth as the value). The MapReduce framework partitions the mapper output by key and then sorts the key-value pair, which group all the activity events and the user record with the same user ID adjacently. The reducer can then perform the actual join logic easily.

Group By
Set up the mappers so that the key-value pairs use the desired grouping key.

Handling skew
A small number of celebrities may have many millions of followers, and such disproportionately active database records are known as linchpin objects or hot keys.
Collecting all activity related to a celebrity in a single reducer can leader to significant skew(hot spots). To compensate it, Pig runs a simpleing job to determine hot keys, and mapper send any records relating to a hot key to one of several reducers. Or specify the hot key explicitly.

***Map-Side Joins*** It makes assumptions about the size, sorting, and paritionning of input datasets.
1. Broadcast hash joins: load the samll join input into an in-memory hash table or sotre the small join input in a read-only index on the local disk. 
2. partitoined hash joins: Bucketed map joins
3. Map-side merge joins

***The output of batch workflows***
It is closer to analytics, but it is not a report, but some kind of structure.
Hadoop MapReduce helps build search indexes, e.g., Lucene/Solr.
Another common use for batch processing is to build machine learning systems such as classifiers and recommendation systems. The output of those batch jobs is often some kind of database.
Build a brand-new database inside the batch job and write it as files to the job's output directory in the distributed filesystem. Those data files are then immutable once written, and can be loaded in bulk into servers that handle read-only queries. Various key-value store support building database files in MapReduce jobs, including Voldermort, Terrapin, ElephantDB, and HBase bulk loading.

***Philosophy of batch process outputs***
Encourage experiments by leaving input unchanged;
Ease of rolling back;
fault-tolerant;
separate logic from wiring.
But Hadoop uses more structured file formats: Avro, Parquet.

Comparing Hadoop to Distributed database
Hadoop provides someting much more like a general-purpose operating system that can run aribitrary programs.
Hadoop opened up the possibility of indiscriminately dumping data into HDFS, and only later figuring out how to process it further.
`sushi principle`: raw data is better.

Hadoop has often been used for implementing ETL process: data from transaction processing systems is dumped into the distributed filesystem in some raw form, and then MapReduce jobs are written to clean up that data, transform it into a relational form, and import it into an MPP data warehouse for analytic purposes.

The Hadoop ecosystem includes both random-access OLTP daabases such as HBase and MPP-style analytic databases such as Impala. Neither uses mapReduce, but both use HDFS for storage.

On top of MapReduce: Hive, Pig, Cascading, Crunch

***Materilzation of Intermediate State***
MapReduce's approach of fully materializing intermediate state has downsides compared to Unix pipes:
1. A MapReduce job can only start when all tasks in the preceding jobs are done, whereas processes connected by a Unix pipe are started at the same time.
2. Mappers are often reduandant
3. Sotring intermediate state in a distributed filesystem means replication over several nodes, which is overkill.

DataFlow Engines: Spark, Tez, and Flink.
Use dataflow engines to implement the same computations as MapReduce workflow, and they are significantly faster. Operators are a generalization of map and reduce.
Workflows implemented in Pig, Hive, or Cascading can be switched from MapReduce to Tez or Spark with a simple configuration change.

Fault tolerant: whether operators are deterministic.

#####Graphs and Iterative processing
Not suitable with MapReduce.

#####Pregel: parallizing graph algorithm

####Summary
in Unix, the uniform interface is files and pipes; in MapReduce, that interface is a distributed filesystem.
Crucially, the input data is bounded: it has a known, fixed size and so a job eventually will be done.


###Chapter 11. Stream Processing
Stream: data that is incrementally made available over time, e.g., stdin and stdout of Unix, programming languages(lazy list), filesystem APIs(java's FileInputStream), TCP connection, delivering audio and video over the internet.

####Transimitting Event Streams
In a stream processing context, a record is more commonly known as an event. An event usually contains a timestamp.
An event is generated once bya producer, and the potentially processed by multiple consumers. In a filesystem, a filename identifies a set of related records; in a streaming system, related events are grouped together into a topic or a stream.

Databases do not support notification mechanism very well: triggers is somewhat of an afterthought. Polling is expensive in database for continual processing with low delays.

***Message Systems***
Publish/subscribe model:
questions:
1. what happens if producers send messages faster than the consumers can process?
    - drop
    - buffer message in a queue
    - apply backpressure or flow control, i.e., blocking the producer from sending more
Unix pipes and TCP use backpressure: when a small fixed-size buffer fills up, the sender is blocked.
2. What happens if nodes crash or temporarily go offline?
Whether message loss is acceptable depends on the application.

Direct Messaging from producer to consumers:
1. UDP multicast: application-level protocols can recover lost packets.
2. Brokerless messaging libraries: ZeroMQ, nanomsg over TCP or IP multicast
3. statsD and Brubeck use unreliable UDP messaging for collecting metrics
4. If the consumer exposes a service on the network, producers can make a direct HTTP or RPC request to push messages to the consumer. This is the idea behind webhooks, a pattern in which a callback URL of one service is registered with another serice, and it makes a request to that URL whenever an event occurs.
Downsides: if the producer breaks down, it loses all the messages in the buffer.

Message Brokers/ message queue
It is essentially a kind of database that is optimized for handling message streams.
It runs a server, with producers and consumers connecting to it as clients.
Message brokers can keep messages in memory or write them to disk, maybe depending on the configuration.
A consequence of queuing is asynchonous consumers.

Message brokers compared to databases:
1. database keep data until it is explicitly deleted; most message brokers delete a message when it has been succesfully delivered to its consumers.
2. message brokers assume their working set is fairly small.
3. databases support secondary indexes and other ways of searching for data; message borkers support subscribing to a subset of topics matching some pattern.
4. when querying a database, the result is based on a point-in-time snapshot; message brokers do not support arbitrary queries, but they do notify clients of the changes.
Message broker standard: JMS and AMQP. 
Implementation: RabbitMQ, ActiveMQ, HornetQ, Qpid, TIBCO Enterprise Message Service, IBM MQ, Azure Service Bus, and Google Cloud Pub/Sub.

Multiple consumers:
two patterns:
1. load balancing
   Each message is delivered to just one consumer.
   useful when the messages are expensive to process
2. Fan-out
   Each message is delivered to all consumers.
   Each message is delivered to all of the consumers.
The two patterns can be combined.

Acknowledgements and redelivery
To prevent loss of a message, message brokers use acks.

#####Partitioned Log
Using Logs for mesage storage
A producer sends a message by appending it to the end of the log, and a consumer receive message by reading the log sequentially.
Apache Kafka, Amazon Kinesis Stream, and Twitter's DistributedLOg are log-based message brokers tha work like this.
The JMS/AMQP broker is preferble with regards to performance, whereas the log-based one works very well for message with strict ordering requirement.

When consumers cannot keep up with producers:
Monitor how far a consumer fall behind the head of the log. As the buffer is large, there is enough time for a human operator to intervene.

Replaying old messages

####Database and Stream

Keep systems in sync
Keep in sync between OLTP, cache, a full-text index, and a data warehouse.

Change data capture: taking data in the order it was written to one database, and applying the changes to other systems in the same order. LinkedIn's Databus, Facebook's Wormhole parse the replication logs.

Log compaction is supported by Apache Kafka, and it allows the message broker to be used for durable storage, not just for transient messaging.

API support for change streams
Database are beginning to support change streams as a first-class interface.

Event sourcing

State, streams, and immutability

Advantages of immutable events:
Auditability, more information than just the current state.

Derving several views from the same event log:
It is entrely reasonable to denormalize data in the read-optimized views, as the translation process gives you a mechanism for keeping it consistent with the event log.

concurrency control

Limitation of immutability
Deletion is matter of "making it harder to retrieve data" than actually "making it impossible to retrieve data".

####Processing Streams
Three options:
1. write it to a database, cache, search index or similar storage system.
2. push to users.
3. produce one or more output streams.

Stream processing has long been used for monitoring purposes.

Complex event processing:
CEP engines reverse the roles of queries and data: queries are long-term, whereas data keep flowing.

Stream Analytics: oriented toward aggregations and statistical metrics over a large number of events. Apache Storm, Spark streaming, Kafka Streams, Google Cloud Dataflow and Azure Stream Analytics.
The time interval over which you aggregate is known as a `window`.

Maintaining materialized views

Search on streams:
full-text search queries: Elasticsearch

Message passing and RPC

***Reasoning about time***
event time vs processing time

Whose clock are you using?

Types of windows:
1. Tumbling window: |   |   |
2. Hopping window: overlap: | |   | |
3. sliding window: ||   ||, keeping a buffer of events sorted by time and removing old events when they expire from the window.
4. session window: for the same user

Stream joins:
1. stream-stream joins (window join)
  Searched-for URL, click-through join.
2. stream-table join(stream enrichment)
3. table-table join(materialized view maintenance)

Fault tolerance
1. microbatching and checkpointing
2. atomic commit revistied: amortize
3. Idempotence
4. rebuilding state after a failure

####summary
Message brokers:
1. AMQP/JMS-style message broker
2. Log-based message broker


### Chapter 12. The future of data systems
How to create applications and systems that are reliable, scalable, and maintainable.

####Data Integration
The most appropriate choice of software tool also depends on the circumstances. Every piece of software, even a so-called "general-purpose" database, is designed for a particular usage pattern.

combing specialized tools by deriving data

reasoning about dataflows

derived data vs distributed transactions

the limits of total ordering

ordering events to capture causality

reprocessing data for application evolution

The lambda architecture

Echo chamber

Surveillance
