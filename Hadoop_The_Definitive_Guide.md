*** Hadoop: The Definitive Guide ***
# Hadoop Fundamentals
## Meet Hadoop
* Data * Big data *
1 terabyte equals 1000 gigabyte.
disk transfer speed increases from 4.4 MB/s to 100 MB/s, but the disk volume grows faster than that. And writing is even slower.

Besides increasing reading and writing throughput, there is more to being able to read and write data in parallel to or from multiple disks. The first one is hardware failure; HDFS takes a different way from RAID. The seocnd one is to be able to combine the data in some day, and data read from one disk may need to be combined with data from any of the other disks. MapReduce provides a programming model that abstracts theproblem from disk reads and writes, transforming it into a computation over sets of keys and values.

Hadoop runs on commodity hardware and is open source.

* Query all your data *
The apprach taken by MapReduce may seem like a brute-force approach. The premise is that the entire dataset--or at least a good part of it--can be processed for each query. MapReduce is an batch query processor.

* Beyond Batch *
For all its strength, Mapreduce is funamentally a batch processing system, and is not suitable for interactive analysis. Queryies usually take miinutes or more, so it's best for offline use.

Hadoop has evolved beyond batch processing. and is sometime used to refer to a larger ecosystem of projects, not just HDFS and MapReduce, that fall under the umbrella of infrastructure for distributed computing and large-scale data processing.

The first component to provide online access was HBase, a key-value store that uses HDFS for its underlying storage. HBase provides both online read/write access of individual rows and batch operations for reading and writing data in bulk.  
The real enabler for new processing models in Hadoop was the introduction of YARN(Yet Another Resource Negotiator) in Hadoop 2. YARN is a cluster resource management system, which alows any distributed program(not just MapReduce) to run on data in a Hadoop cluster.

Different processing patterns:
1. Interactive SQL:
by dispensing with MapReduce and using a distributed query engine that uses dedicated "always on" daemons(like impala) or container reuses(like Hive on Tez), it's possible to achieve low-latency response for SQL queries on Hadoop while scaling up to large dataset sizes.

2. Interactive processing
Many algorithms-such as those in machine learning- are interactive in nature, so it's much more efficient to hold each intermediate working set in memory, compared to loading from disk on each interation. The architecture of MapReduce does not allow this, but it's straightforward with Spark.

3. Stream processing
Storm, Spark streaming, or Samza make it possible to run real-time, distributed computations on unbounded streams of data and emit results to Hadoop stoage or external systems.

4. Search
The Solr search platform can run on a Hadoop cluster, indexing documents as they are added to HDFS, and serving search queries from indexes stored in HDFS.

* Comparision with other systems *
1. RDBMS:
Disk seek time is improving more slowly than tranfer rate. Seeking is the the process of moving the disk's head to a particular place on the disk to read or write data. It characterizes the latency of a disk operation, whereas the transfer rate correspond to a disk's bandwidth.
If the data access pattern is dominated by seeks, it will take longer to read or write large portions of the dataset than streaming through it, which means MapReduce is faster than RDBMS. On the other hand, for updating a small portion of records in a database, a traditional B-Tree works well, where B-tree is used in relational databases, which is limited by the rate at which it can perform seeks. For update the majority of a database, a B-Tree is less efficien than MapReduce, which uses Sort/Merge to rebuild the database.

In may ways, MapReduce can be seen as a complement to a RDBMS. MapReduce is a good fit for problems that need to analyze the whole dataset in a batch fashion, particularly for ad hoc analysis. An RDBMS is good for point queries or updates, where the dataset has been indexed to deliver low-latency retrieval and update times of a relatively small amount of data. MapReduce suits applications where the data is written once and read many times, whereas a relational database is good for datasets that are continually updated.

        Traditional RDBMS       MapReduce
Data size   GigaBytes               PetaBytes
Access      Interactive and batch   Batch
Updates     Read and write many times   Write once, and read many times
Transactions    ACID                None
Structure       Schema-on-write     Schema-on-read
Integrity       High                Low
Scaling         Non-linear          Linear

However, the differences between relational databases and Haddop systems are blurring.
Structure vs non-structure: XML or dataset tables vs a spreadsheet or plain text or image data. Non-structure provides flexiblity and avoid costly data loading phase of an RDBMS, since in Hadoop it is just a file copy.
Relational data is often normalized to retain its integrity and remove redundancy. Normalization poses problems for Haddop processing because it makes reading a record a nonlocal operation, and one of the central assumptions that Hadoop makes is that it is possible to perform(high-speed) streaming reads and writes.

2. Grid computing
The high-performance (HPC) computing and grid computing commuinites have been doing large-scale data processing for years, using such application program interfaces(APIs) as the Message Passing Interface(MPI). The approach in HPC is to distribute the work across a cluster of machines, which access a shared filesystem, hosted by a storage area network(SAN). But the network bandwidth is the bottleneck.
Hadoop tries to co-locate the data with the compute nodes, so data access is fast because it is local. This feature, `data locality`, is at the heart of data processing in Hadoop and is the reason for its good performance.
Processing in Hadoop operates only at the higher level: the programmer thinks in terms of the data model, which the date flow remains implicit.
MapReduce is able to handle partial failure because it is a `shared-nothing` architecture, meaning that tasks have no dependence on one other. So from the programmer's point of view, the order in which the tasks run doesn't matter.

3. voluteer computing
SETI@home project: search for extra-terrestrial intelligence.
MapReduce is designed to run jobs that last minutes or hours on trusted, dedicated hardware running in a single data center with very high aggregate bandwidth interconnects. By ocntrast, SETI@home runs a perpetual computation on untrusted machines on the Internet with highly variable connection speeds and no data locality.

* A brief history of hadoop *
Hadoop was created by Doug Cutting, the creator of Apache Lucene, the text search library. Haddop has its origins in Apache Nutch, an open source web search engines, itself a part of the Lucene project.
Hadoop is a made-up name.
Hadoop gets it idea from a paper about GFS, Google's distributed filesystem, and also MapReduce.
Search consists of four components: Crawler to download pages, WebMap to build a graph of the known web, indexer to build a reverse index to the best pages, and Runtime to answer user's queries.

Hadoop's role as a general-purose storage and analysis platform for big data has been recognized by the industry.


## Chapter 2. MapReduce
MapReduce is a programming model for data processing, and it is inherently parallel.
MapReduce works by breaking the processing into two phases: map and reduce. Each phase has key-value pairs as input and output, the type of which may be chosen by the programmer. The programmer also specifies two functions: the map function and reduce function.
e.g., the map function: the key is the offset of the line from the beginning of the file. The map function merely extracts the year and the air temperature, and emits them as its output. The output from the map function is processed by the MapReduce framework before being sent to the reduce function. The processing sorts and groups the key-value pairs by key.

input | map | sort shuffle | reduce > output

* Java MapReduce *
define `map` function in `Mapper` class. `Mapper` class is a generic type with four formal type parameters that specify the input key, input value, output key, and output value types of the map function. The `reduce` function is defined using a `Reducer`.
The third piece of code to run the MapReduce job: define job, set input/output path, set mapper/reducer classes, set key/value classes.

* Scaling out *
To scale out, we need to store the data in a distributed filesystem(HDFS). This allows Hadoop to move the MapReduce compuatation to each machine hosting a part of the data, using Hadoop's resource management system, YARN.

* Data Flow *
A MapReduce `job` is a unit of work consisting of input data, the MapReduce program, and configuration information. Hadoop runs the job by dividing it into `tasks`: map and reduce. If a task fails, it will be rescheduled to run on a different node.
Hadoop divides the input into fixed-size pieces called `input split`, or `split`. Hadoop creates one task for each split. On one hand, the quality of the load balancing increases as the splits become more fine grained; on the other hand, if splits are too small, the overhead of managing the slits and map tasks creation begins to dominate the total job execution time. For most jobs, a good split size tends to be the size of an HDFS block, 128 MB by default.
`data locality optimization`: Hadoop does its best to run the map task on a node where the input data resides in HDFS.
Map tasks write their output to the local disk, not to HDFS. Because it is intermediate output and will be thrown away once it is processed by reduce task.
Reduce tasks don't have the advantage of data locality: the input to a single reduce task is normally the output from all mappers. Therefore, the sorted map outputs have to be transferred across the network to the node where the reduce task is running, where they are merged and then passed to the user-defined reduce function. The output of reduce is stored in HDFS for reliability.
Combiner function can be sued to minimize the data transferred between map and reduce task.

* Hadoop Streaming *
Hadoop provides an API to MapReduce that allows you to use languages other than java to write your map and reducer functions. It uses Unix standard streams as interface between Hadoop and your program.
The number of reducer tasks is not governed by the size of the input, but instead is specified independently. When there are multiple reducers, the map tasks partition their output, each creating one partition for each reducer task.
