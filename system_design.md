# System Design
Leaning how to design scalable systems will help you become a better engineer

### Index of system design topics
client, dns, cdn, load balancer, web server,, read api, write api, search api, timeline service, tweet info service, fan out service, user graph service, search service, notification service, memory cache, sql read replicas, sql write master-slave, object store.

***Everything is a trade-off.***

### Scalability
Vertical scalability: more cpu, more ram, more disk...., SSD > SAS > SATA
Horizontal scalability: more servers in data center.
load balancing with load balancer, a better choice.
loading balancing with BIND(dns round robin), considering caches in local computer.

Session: centralized session sharing
RAID for disk redundancy. RAID 0, 1, 5, 6, 10
Shared storage: FC, iSCSI, NFS, MYSQL
Load Balancers:
- Software: ELB, HAProxy, LVS
- Hardware: Barracuda, Cisco, Citrix, F5
cookie: put a random number in cookie to map to a specific web server in Load Balancer. Load Balancer can set cookie in set-cookie header.
PHP accelerator: cached opcode, like pyc, pyo file in python.

Caching: html(static but fast), MySQL(enable cache), memcache(Store everything in RAM, key-value store),

Database storage engine: innoDB(support transaction), ISAM(use locks), Archive(compress, good for log files),

Database replication feature: Master-Slave, balancing read request. Single-master, master-master, multi-tier architecture: load balancer->read query. A pair of load-balancers.

Partition: A-M cluster, N-Z cluster.

HA: high availability

Sticky sessions:

security: only open ports needed, e.g. tcp 80, 443 for http and https, tcp 3306 for mysql.

### Scalability for dummies
clones: "outsourcing" you sessions to external database or persistence cache, like Redis, and serving the same codebase from all your servers.
Database: Master-slave, or denormalize from the beginning and include no more Joins in any database query, or switch to NoSQL database.
Cache: in-memory cache like Memcached or Redis. Never do file-based caching. A cahe is a simple key-vaue store and it should reside as a buffering between your applicatoin and your data storage. 1. Cached Database queries. 2. Cached objects. REdis has extra database-features, like persistence and the built-in data structures like lists and sets. With Redis and a clever keying there may be a chance you even can get rid of a database. But Memcached scales like a charm.
Asynchronism:
- Precomputing: generate html beforehand.
- MessageQueue: rabbitmq, celery


## Tradeoff
### Performance vs Scalability
A service is scalable if it reuslts in increased performace in a manner proportioal to resources added. Generally, increasing performance means serving more units of work, but it can also handle larger units of wwork, such as when datasets grow.
If you have a performance problem, your system is slow for a single user.
If you have a scalability problem, your system is fast for a single user, but slow under heavy load.
### Latency vs thoughput
Latency is the time to perfomr some action or to produce some result.
Throughput s the number of such actions or results per unit of time.
Aim for maximal throughtput with acceptable latency.
### Availability vs consistency
CAP theorem
- Consistency: every read receives the most write or an error
- Availability: every request recieve a response, without guarantee that it contains the most recent version of the information.
- Partition Tolerance: the system continues to operate despite arbitrary partitioning due to network failures.
Networks aren't reliable. so partition tolerance is required. You'll need to make a software tradeoff between consistency and availability.
#### CP - consistency and partition tolerance
Waiting for a response from the partitioned node might result in a timeout error. CP is a good choice if your business needs require atomic reads and writes.
#### AP - availability and partition tolerance
Responses return the most recent version of the data availability on the node, which might not be the latest. Writes might take some time to propagate when the partition is resolved.
AP is a good choice if the business needs allow for eventual consistency or when the system needs to continue working despite external errors.

### Consistency Patterns
- weak consistency
  After a write, reads may or may not see it. A best effort approach is taken. A best effort approach is taken. The apporach is seen in systems such memcached. Weak consistency is seen in system such as memcached. Weak consistency works well in real time use cases such as VoIP, video chat, and realtime multiplayer games. For example, if you are on a phone call and lose reception for a few seconds, when you regain connection you do not hear what was spoken during connection loss.
- Eventual consistency
  After a write, reads will eventually see it(typically within milliseconds). Data is replicated asynchronously. The approach is seen in system such as DNS and email. Eventual consistency works well in highly available systems.
Eventual consistency with a run around clerk
- Strong consistency
  After a write, reads will see it. Data is replicated synchronously. This approach is seen in file systems and RDBMSes. Strong consistency works well in systems that need transactions.

### Availability patterns
- Fail-over
    - Active passive: heartbeats are sent between the active and the passive server on the standby. If the heartbeat is interrupted, the passive server takes over the ative's IP address and resume service. 'hot' or 'cold' standby. It can be also referred to as master-slave fail-over.
    - Active-active: Both servers are managing traffic. Each one has its IP address. It can be also referred to as master-master servers.
Disadvantage: 1. add more hardware and complexity. 2. There is a potential for loss of data if the active system fails before any newly written data be replicated to the passive.
- Replication
    - master-slave replication
    - master-master replication

### Domain name system
DNS is hierarchical, with a few authoritative servers at the top level. YOu router or ISP provides information about which DNS servers to contact when doing a lookup. Low level DNS cache mapping, which could become stale due to DNS propagation delays. DNS results can alos be cached by your browser or OS for a certain period of time, determined by TTL.
- NS (name server)
- MX (mail exchange)
- A (name to IP)
- CNAME (canonical, points a name to another name)
Some Managed DNS service can route traffic through:
- Weighted round robin
- Latency-based
- Geolocation-based
Disadvantage:
- A slight delay, although mitigated by caching
- Complex management
- DDos Attack.

### Content delivery network
A content delivery network(CDN) is a globally distributed network of proxy servers, serving content from location closer to the user. Generally, static files such as HTML/CSS/JS, photos, and videos are served from CDN, although some CDNs such as Amazon's CouldFron support dynamic content. The site' DNS resolution will tell clients which server to contact.
Serving content from CDN can significantly improve performance in two ways:
- User receive content at data centers close to them
- Your servers do not have to server requests that CDN fulfills.
####Push CDNs
Push CDNs receive new content whenever changes occurs on your server. You take full responsiblity for providing content, uploading directly to the CDN and rewriting URLs to point to the CDN. You can configure when content expires and when it is updated. Content is uploaded only when it is new or changed, minimizing traffic, but maximizing storage.
Sites with a small amount of traffic or sites with content that isn't often updated work well with CDN. Content is placed on the CDN once, instead of being re-pulled at regular intervals.
####Pull CDNs
Pull CDNs grab new content from your server when the first user requests the content. You leave the content on your server and rewrite URLs to point to the CDN. This results in a slower request until the content is cached on the CDN.
A time-to-live(TTL) determines how long content is cached. Pull CDNs minimize storage space on the CDN, but can create redudant traffic if files expire and are pulled before they have actually changed.
Sites with heavy traffic work well with pull CDNs, as traffic is spread out more evenly with only recently-requested content remaining on the CDN.
Disadvantages: CDN
- costs could be significant depending on traffic
- Content might be stale if it is updated before the TTL expires it.
- CDNs require changin URL for static content to point to the CDN.

### Load Balancer
1. Pick a worker to forward request to computing resources such as application servers and databases.
    - Random
    - Round robin
    - Least busy
    - Sticky session/cookies
    - By request parameters
2. Wait for its response
3. forward the response to client

Load balancer is effective at:
- Preventing requests from going to unhealthy servers
- Preventing overloading resources
- Helping eliminate single points of failure.
- SSL termination:; decrypt requests and encrypt responses and remove the need to install X.509 certificates on eah server.
- Session persistence: issue cookies and route a specific client's requests to the same instance if the web apps do not keep track of sessions

To protect against failures, set up multiple load balancers, either in active-passive or active-active.
Load balancers can route traffic based on varous metrics, including:
- Random
- Least loaded
- Session/cookeis
- Round robin or weighted round robin
- layer 4: look at info at transport layer to decide how to distribute reqeust. Generally, this involves the source/destination IP addresses, and ports in the header, but not the contents of the packet. It performs NAT(Network Address translation)
- layer 7: look at the application layer to decide how to distribute requests. It terminates network traffic, reads teh messgae, make a load-balancing decision, then opens a connection to the selected server. e.g., it can direct video traffic to servers that host videos while directing more sensitive user billing traffic to security-hardned servers.
At the cost of flexiblity, layer 4 requires less time and computing resources than layer 7.

Load balancers can help with horizontal scaling, improving performance and availability. Scaling out using commodity machines is more cost effetive and results in higher availability than scaling up a single server on more expensive hardware, called vertical scaling. It is also easier to hire for talent working on commodity hardware than it is for specialized enterprise systems.
Disadvantage: horizontal scaling
- introduces complexity and involves cloing servers
    - servers should be stateless: they should not contain any user-related data like sessions or profile pictures.
    - Sessions can be stored in a centralized data store such as a database or a persistent cache(Redis, Memcached)
- Downstream servers such as cachees and databases need to handle more simultaneous connections as upstream servers scale out.
Disadvantage: load balancer
- The load balancer can become a performance bottleneck if it does not have enough resources or if it is not configured properly
- Introducing a load balancer to help eliminate single points of failure results in increased complexity.
- A single load balancer is a single point of failure, configuring mulitple load balaners further increase complexity.

### Reverse proxy(web server)
A reverse proxy is a web server that centralized internal services and provides unified interface to the public. Requests from clients are forwarded to a server that can fulfill it before the reverse proxy returns the server's reponse to clients.
Additional benefits include:
- Increased security: hide information about backend servers, blacklist IPs, limiting numberof connection per client
- INcreases scalability and flexibilty: Clients only see the revrese proxy's IP, allowing you to scale servers or change their configuration.
- SSL termination
- Compression
- Caching
- Static content: serve static content directly.

Load balancer vs reverse proxy
- Deploying a load balancer is useful when you have multiple servers. Often, load balancers route traffic to a set of servers serving the same function.
- Reverse proxies can be useful even with just one web server or application server, opening up the benifitf described in above.
- Solutions such as NGINX and HAProxy can support both layer 7 reverse proxying and load balancing.
Disadvantage:
- Increased complexity.
- multiple reverse proxies, increased complexity

## Application layer
Separating out the web layer from the application layer(also known as platform layer) allow you to scale and configure both layers independently. Adding a new API results i adding application servers without necessary adding additional web servers.
The `single responsiblity principle` advocates for small and autonomous services that work together. Small teams with small services can plan more aggressively for rapid growth.
Workers in the application layer also help enable asynchronism.

### Microservices
A suite of independently deployable, small, modular services. EAch service runs a unique process and communicates through a well-defined, lightweight mechanism to server a business goal.
Pinterest, for example, could have the following microservices: user profile, follower, serach, photo upload, etc.

### Service Discovery
Systems such as Consul, Etcd, and Zookeeper can help services find eachother by keeping track of registered names, addresses, and ports. Health checks help verify service integrity and are often done using an HTTP endpoint. Bothe Consul and Etcd have a bulit-int key-value store that can be useful for storing config values and other shared data.
Disadvantage: application layer
- Adding an application layer with loosely coupled services requires a different approach from an architectura, operations, and process viewpoint (vs a monolithic system)
- Microservices can add complexity in terms of deployments and operations.
SOA - service oriented architecture

### Database
Relational database management system(RDBMS)
A relational database like SQL is a collection of data items organized in tables.
ACID:
- Atomicity: each transaction is all or nothing.
- Consistency: Any transaction will bring the database from one valid state to another.
- Isolation: Executing transaction concurrently has the same results as if the transactions were executed serially.
- Durability: Once a transcation has been committed, it will remain so.

May techniques to scale up a relational database:
- Master-slave replication
  The master servers reads and writes, replicating writes to one or more slaves, which serve only reads. If the master goes offline, the system can continue to operate in read-only mode until a slave is promoted to a mster or a new master is provisioned.
Additional logic is needed to promote a slave to a master.
- Master-master replication
  Both masters server reads and writes and coordinate with each other on writes. If either goes down, the other will continue to operate with both reads and writes.
Disadvantage: 1. Need a load balancer or you'll need to make changes to your applicatoin logic to determine which one to write. 2. Most are either loosely consistent or have increased write latency due to synchroniztion. 3. Conflict resolution comes more into play as more writes nodes are added and as latency increases.
  Disadvantages: replication
    - There is a potential for loss of data if the master fails before any newly written data can be replicated to other nodes.
    - Writes are replayed to the read replicas. If there are a lot of writes, teh read replicas can get bogged down with replaying writes adn can't do as many reads.
    - The more read slaves, the more you have to replicate, which leads to greater replication lag.
    - On some systems, writing to the master can spawn mulitple threads to write in paralle, whereas read can only support writing sequentially with a single thread.
- Replication adds more hardware and additional complexity.

- Federation
  Federation(or functional partitioning) splits up databases by function. Instead of a single, monolithic databse, you could have several databases, resulting less read and write traffic to each databaseand and therefore less replication lag. Smaller databases result in more data that can fit in memory, which in turn results in more cache hits due to improved cache locatlity. With no single central master serialzing writes, you can write in paralle, increasing throughput.
  Disadvangtage: 1. not effective if your schema requires huge functions or tables. 2. need to update your applicatoin logic to determine which database to read and write. 3. Joining data from tow databases is more complex with a server link. 4. Federation adds more hardware and additional complexity.

- Sharding
  Sharding distributes data across different databases such that each database can only manager a subset of the data. Similar to the advantages of federation, sharding results in less read and write traffic, less replication, and more cache hits, faster query, increased write throughput.
  Common ways to shard a table of users is either through the user's last name initial or the user's geographic location.
  Disadvantage: 1. need to update appliatoin logic to work with shards; 2. Data distribution can become lopsided in a shard, for example, much of power users in one shard; 3. Joining data from mulitiple shards is more complex. 4. more hardware and additional complexity.

- Denormalization
  Denormalization attempts to improve read performance at the expense of some write performance. Redundancy data are written in multiple tables to avoid expensive joins. Some RDBMS such as PostgreSQL and Oracle support materialized views which handle the work of storing redundant information and keeping reduendant copies consistent.
  Once data becomes distributed with federation or sharding,managing joins across data centers further increases complexity. Denormalization might circuemvent the need for such complex joins. In most systems, reads can heavily outnumber writes 100:1 or even 1000:1. A read resulting in a complex database join can be very expensive, spending a significant amount of time on disk operations.
 Disadvantage: 1. Data is duplicated. 2. Constraints can help redundant copies of information stay in sync, which increases complexity of the database design. 3. a denormalized database under heavy write load might perform worse than its normalized counterpart.

- SQL tuning
  It's important to benchmark and profile to simulate and uncover bottlenecks.
  optimizations:
    - Tighten up the schema:
        - MYSQL dumps to disk in contiguous blocks for fast access.
        - Use CHAR instead of VARCHAR for fixed-length fields. CHAR effectively allow for fast, random access, whereasa with VARCHAR, you must find the end of a string before moving onto the next one.
        - Use TEXT for large blocks of text such as blog posts. TEXT also allows for boolean searches. Using a TEXT field results in storing a pointer on disk that is used to locate the text blog.
        - Use INT for larger numbers up to 2^32.
        - Use DECIMAL for currency to avoid floating point representation errors.
        - Avoid storing large BLOBS, store the locatoin of where to get the object instead.
        - VARCHAR(255) is the largest number of characters that can be counted in an 8 bit number, often maximizing the use of a byte in some RDBMS.
        - Set the NOT NULL constraint where applicable to improve serach performance.
    - Use good indices:
        - Columns that you are querying could be faster with indics.
        - indices are usually represented as self-balancing B-tree that keeps data sorted and allows searches, sequential access, insertions, and deletions in logarithmic time.
        - Placing an index can keep the data in memory, requiring more space.
        - Writes could also be slower since the index also needs to be updated.
        - When loading large amounts of data, it might be fastetr to disable indeices, load the data, then rebuild the indices.
    - Avoid expensive joins: denormalize where performance demands it.
    - Partition tables: break up a table by putting hot spots in a seperate table to help keep it in memory.
    - Tune the query cache.

### NoSQL
NoSQL is a collection of data items represented in a key-value store, document-store, wide column store, or a graph database. Data is denormalized, and joins are generaly done in the application code. Most NoSQL stores lack true ACID transactions and favor eventual consistency.
BASE is often used to describe the properties of NoSQL databases. In comparison with the CAP theorem, BASE chooses availability over consistency.
- Basically available; guarantees availability.
- Soft state: the state of the system may change over time, even without input.
- Eventual consistency - the system will become consistent over a period of time, given that the system doesn't receive input during that period.

#### Key-value store
Redis, Memcached
Abstraction: hash table
A key-value store generally allows for O(1) reads and writes, and if often backed by memory or SSD. Data stores can maintain keys in lexicographic order, allowing efficient retrival of key ranges. Key-value stores can allows for storing metadata with a value.
Key-value stores provide high performance and are often used for simple data models or for rapidly-changing data, such as an in-memory cache layer. Since they offer only a limited set of operatons, complexity is shifted to the application layer if additional operations are needed. It is the basis for more complex systems such as a document store, and in some cases, a graph database


#### Document store
MongoDB, CouchDB, Elasticsearch
Abstraction: key-value store with documents stored as values
A document store is centered around documents(XML, JSON, binary, etc), where a docuemtn stores all information for a given object. It provides APIS or a query language to query based on the internal structure of the document itself.
Based on the underlying implementation, documents are organized in either collectoins, tags, metadata, or directories. Although documents can be organized or grouped together, document may have fields that are completely different from each other.
MongoDB and CouchDB also provides a SQL-like language to perform complex queries. DynamoDB supports both key-values and documents.
Document stores provide high flexibility and are often used for working with occasionally changing data.

### Wide column store
Abstraction: nested map ColumnFamily<Rowkey, columns<Colkey, value, timestamp>>
A wide column store's basic unit of data is a column(name/value pair). A column can be grouped int column families(analogous to a SQL table). Super column families further group column families. You can access each column independently with a row key, and columns with the same row key form a row. Each value contains a timestamp for vresioning and for conflict resolution.
Google introduced Bigtable as the first wide column store, which influenced the open-source HBase often used in Haddop ecosystem, and Cassandra from Facebook. Stores such as BigTable, Hbase, and Cassandra maintain keys in lexicographic order, alloing efficient retrieval of selective key ranges.
Wide column stores ofer hig availability and high scalability. They are often used for very large data sets.

### Graph database
Neo4j, FlockDB
Abstraction: graph
In a graph database, each node is a record and each arc is a relationship between two nodes. Graph databases are optimized to represetn complex relationsihps with many foreign keys or many-to-many relationships.
It ofers high performance for data models with complex relationships, such as network. They are relatively new and are not yet WIDELYsed; it might be more difficult ot find development tools and resources. Many graphs can only be accessed with REST APIs.

### SQL or NoSQL
Reasons for SQL:
- strutured data
- strict schema
- relational data
- need for complex joins
- transactions
- clear patterns for scaling
- More established: developers, community, code, tools, etc.
- Lookups by index are very fast
Reasons for NoSQL:
- Semi-structured data
- Dynamic or flexible schema
- Non-relational data
- No need for complex joins
- Store many TB(or PB) of data
- Very data intensive workload
- Very high throughput of IOPS
Sample data well-suited for NoSQL
- rapid ingest of clickstream and log data
- leaderboard or scoring data
- Temporary data, such as a shopping cart
- Frequently accessed tables
- Metadata/lookup tables

### Cache
Caching improves page load times and can reduce the load on your servers and databases. In this model, the dispathcer will first look up if the request has been made before and try to find the previous result to return, in order to save the actual execution. This is cache on the dispatcher.
- Databases often benefit from a uniform distribution of reads and writes across its partitions. Popular items can skew the distribution, causing bottlenecks. Putting a cache in front of a database can help absorb uneven loads and spikes in traffic.

- Caching can be located on the client side(OS or browser), server sides, or in a distinct cache layer.
CDN are considered a type of cache.
- Web server caching: Reverse proxies and caches such as Varnish can serve static and dynamic content directly. We server can also cache requests, returing responses without having to contact application servers.
- DataBase caching: You database usually includes some level of caching in a default configuratoin, optimized for a generic use case. Tweaking these settings for specific usage patterns can further boost preformance.
- Application caching: 
  in-meomry caches such as Memcached and Redis are key-value stores between your applicatoin and your data storage.
  There are multiple levels you can cache: database queries and objects. Suggestions to caches at the object level: user sessions, fully rendered web pages, activity streams, user graph data.

#### When to update the cache
- Cache aside
  The application is responsbile for reading and writing from storage. The cached does not interact with storage directly. The application looks for entry in cache, resulting in a cache miss, load entry from the database, add entry to cache, and return entry. Memcached is used in this manner. Cache-asdie is also refer to lazy loading.
  Disadvantage: 1. each cache miss results in three trips, which can cause a noticable delay. 2. Data can become stale if it is updated in teh database. This issue is mitigated by setting a time-to-live which forces an update of the cache entry, or by using write-through. 3. When a node fails, it is replaced by a new, empty node, increasing latency.
- Write-through
  It uses the cache as the main data store, reading and writing data to it, while teh cache is responsible for reading and writing to the database. Applicatoin adds/update entry in cache, and cache synchronously writes entry to data store, and return.
  Disadvantage: 1. when a new node is craed due to failure or scaling, the new node will not cache entries until the entry is updated in the database. Cache-aside in conjuction write through can mitigate this issue. 2. Most data written might never read, which can be minimized with a TTL.
- Write-behind(write-back)
  The application add/update entry in cache, and asynchronously write entry to the data store, improving write performance.
  Disadvantage: 1. There could be data loss if teh cache goes down prior to its content hitting the data store. 2. It it more complex to implement wirte-behind than it is to implemtn cache-aside or write-through
- Refresh-ahead
  Configure the cache to automatically refresh any recently accessed cache entry prior to its expiration. It can result in reduced latency vas read-through if the cache can accuratel predict whic items are likely to be needed in the future.
  Disadvantage: Not accrately predicting which items are likely to be needed in the future can result in reduced perfoamnce than without refresh-ahead.

Disadvantages(cache):
- Need to maintain consistency between caches and the source of truth as the database through cache invalidation.
- Need to make application changes such as adding Redis or memcached.
- Cache invalidation is a difficult problem, there is additional complexity associated with when to update teh cache.

### Asynchronism
Asynchronous workflows help reduce request times for expensive operations that would otherwise be pefomred in-lin. They can alos help by doting time-consuming work in advance, such as periodic aggregation of data.
- Message queue
  It receive, hold, and deliver meesages. If an operatoin is too slow to perform inline, you can use a message queue with the following workflow: an application publishes a job to the queue, then notifies the user of job satus; a worker pickes up the job from the queue, process it, then signal the job is done. The use is not blocked and the job is processed in the background. if posting a tweet, the tweet could be instaly posted to your timeline, but it could take sometime before your tweet is actually delivered to all of your followers.
  Redis is useful as a simple message broker but messages can be lost.
  RabbitMQ is popular but requires you to adapt to the 'AMPQ' protocol and manage your nodes.
  Amazon SQS, is hosted, but can have high latency, and has the possibility of messages being delivered twice.
- Task queues
  Task queues receive tasks and their related data, runs them, then delievers their results. They can support scheduling and can be sued to run computationally-intensive jobs in the background. Celery has support for scheduling and primarily has python support.
- Back pressure
  If queues tart to grow significantly, the queue size can become larger than memory, resuling in cache misses, disk reads, and even slower performance. Back pressure can help by limiting the queue size, thereby maintaining a high throughput rate and good response times for job already in the queue. Once the queue fills up, client get a server busy or HTTP 503 status code to try later.
Disadvantage: use cases such as inexpensive calculations and realtime workflows might be btter suited for synchronous operations, as introducint queues can add delays and complexitity.

### Communication
1. Application: End User Layer, program that opens what was sent or creates waht is to be sent; SMTP, user application; servers as the window for users and application processes to access the network services.
2. Presentation: Formats the data to be presetned to the application layer. It can be viewed as the Translator for the network. Syntax layer; encrypt & decrypt(if needed); Character code transaltion, data conversoin, data compression, data encryption, character set translation; JPEG/ASCII/TIFF?GIF; PICT
3. Session: allow session establishment between processes running on different stations. Synch & send to ports(logical ports) Sessio establishment, maintenance and termination, sesson support, perform security, name recognition, logging, etc. Logical ports; RPC/SQL/NFS/NetBIOS names
4. Transport: ensure that message are delivered error-free, in sequence, and with no losses or duplications. TCP host to host, flow control; meesage segmentation, message acknowldegement, message traffic control, sesson multiplexing, TCP/SPX/UDP
5. Network: controls the operatonsof the subnet, deciding which physical path the data takes. packets, routing, subnet traffic control, frame framentation, logical-physical address mapping, subnet usage accounting. IP/IPX/ICMP, Routers
6. Data Link: provide error-free transfer of data frames from one node to another over the physical layer. Frames. Establish & termiantes teh logical link between nodes. Frame traffic control/sequencin/acknowledgement/delimiting/error checking/media access control. Switch/Bridge/WAP, PPP/SLIP
7. Physical: concerned with the transmission and recpetion of the unstructured raw bit stream over the physical medium. Physical structure calbes, hubs, etc.

### Remote Procedure Call(RPC)
in an RPC, a client cause a procedure to execute on a different address space, usually a remote server. The procedure is coded as if it were a local prodecure call, abstracting away the details of how to communicate with the server from the client. Popular RPC frameworks include Protobuf, Thrift, and Avro.
RPC is a qrequest-reponse protocol:
1. client program- calls the client stub prodecure. The parameters are pushed onto the stack like a local prodecure call. 
2. Client stub prdecure- marshals(packs) procedure id and arguments into a request message. 
3. Client communication module: OS sends the message from teh client to the server.
4. Server communicaton module: OS passes the incoming packets to the server stub procedure. 
5. Server stub procedure: Unmarshals the results, calls the server procedure matching the procedure id and passes the given arguments. 
6. The srever response repeats the steps above in reverse order.
RPC is focused on exposting behaviors. RPCs are often used for performance reasons with internal communications, as you can hand-craft native calls to better fit your use cases.
Choose a SDK when:
- Know your target platform
- want to control how your "logic" is accessed
- want to control how error control happens off your library
- performance and end user experience is you primary concern.
Disadvantage: 
- RPC clients become tightly coupled to the service implementation.
- A new API must be defined for every new operation or use case
- It can be difficult to debug RPC.
- You might not be able to leverage existing tech out of the box. such as, cahcing servers Squid.

### Representational state transfer(REST)
REST is an architectural syel endorcing a client/server model where the client acts on a set of resources managed by the server. All communication must be stateless and cacheable.
There are four qualities of RESTful interface:
- identify resources(URI in HTTP) - use the same URI regardless of any opreation
- Change with represtations(Verbs in HTTP) - use uverbs, headers, and body.
- Self-descriptive error message(status response in HTTP)- use status codes, don't reinvent the wheel.
- HTML interface for HTTP - your web service should be fully accessible in a browser.
REST is focused on exposing data. It minimizes the coupling between client/server and is often used of public HTTP APIs.
Disadvantage:
- It might not be a good fit if resources are not naturally organized or accessed in a simple hierarchy.
- relies on a few verbs which sometimes doesn't fit your use case.
- Fetching complicated resources with nested hierarchies requires multiple round trips between client and server to render single views.
- Over time, more fields might be added to an API repsonse and old clients receive all new data fields, even those that they do not need.s a result, it blats the payload size and leads to large latencies.

### Bloom Filters vs Hash map or other dats structures for represeting sets
Bloom filters have a strong space advantge over other data structure for
represeting sets. A bloom filter doesn't store the elements themselves. You
don't use the bloom filter to test if an element is present, you use it to test
whehter it's surely not present, since it guarantees no false negatives.
Have the filter in the RAM, and use it along with hashmap to check whether an
element exisits.

## Notes:
Memcache is a look-aside cache, which means in case of a cache miss, you hav
e to populate the entry by yourself, different from look-through/cache-through
cache.
