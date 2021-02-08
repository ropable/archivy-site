---
date: 02-05-21
id: 8
path: Books
tags: []
title: Designing Data-Intensive Applications
type: note
---

# Chapter 1 - Reliable, Scalable and Maintainable Applications

* Applications have *functional requirements* (e.g. store data, allow it to be searched, etc.) and //non-functional requirements// (e.g. security, performance, reliability, etc.)
* *Reliability* means making systems work correctly, even when faults occur. Important for this: minimise opportunities for error, test thoroughly, aim for fast recovery, set up good monitoring.
* *Scalability* means having strategies for keeping performance good even as load increases. Important for this: measure and benchmark performance, profile cheap/expensive operations, consider architecture design.
* *Maintainability* means to make life easier for the operations team who need to keep the system running. Important for this: manage complexity through abstraction, make change easy & common, set up good monitoring.

# Chapter 2 - Data Models and Query Languages

Data is represented in models. Historically, data was represented as one big tree, but that wasn't good at representing M2M relationships so we invented *relational models* to solve the problem. Then we found that some applications don't fit well in the relational model either so now we have *NoSQL datastores*, which generally don't enforce a schema on the data they store. NoSQL stores have diverged in two main directions:

* *Document databases* - data comes in self-contained documents, and relationships between documents are rare.
* *Graph databases* - not unlike the above, but anything can relate to anything else.
Most applications still assume that the data has a certain structure; it may be that the schema is explicit (enforced on write) or implicit (handled on read). Every data model comes with its own query language or framework to get the data back out of it. All three models are widely used, are good at their own domain, and each can also be (awkwardly) used to emulate the other models.

# Chapter 3 - Storage and Retrieval

Database storage engines fall into two broad categories: those optimised for online transaction processing (OLTP) and those optimised for online analytics processing (OLAP). They are built for difference usage patterns:

* OLTP systems are typically user-facing and see a large volume of requests. Applications usually only touch a small number of records in each query. Applications query records by some kind of key, and the storage engine uses an index to find the data. Disk seek times are often the bottleneck.
* OLAP systems (data warehouses) are mainly used by business analysts. They usually see a small number of requests, but each query touches a large number of records. Disk bandwidth (not seek time) is often the bottleneck.

OLTP engines mainly use two sorts of storage engines:

* Update-in-place, which treats the disk as a set of fixed-size pages that can be overwritten. B-trees are the most common example of this, used in all major relational databases.
* Log-structured, which only permits appending to files and deleting obsolete files, but never updates a file. This is a comparatively recent design. The key idea is to turn random-access writes into sequential writes on disk, which enables higher throughput.

For workloads used by analytic systems, queries normally require sequentially scanning across many rows so indexes are less relevant  compared to encoding the data very compactly to minimise the amount of data that the query needs to read from disk. Column-oriented storage can help achieve this goal.

As a developer/architect, it's worth knowing something about the internals of storage engines to understand which tool is best suited to your particular workload.

# Chapter 4 - Encoding and Evolution

There are many ways to turn data structures into bytes on the network or disk:

* Language-specific encodings are restricted to a single programming language, but may be efficient for a use case.
* Text formats like JSON, XML and CSV are common and (relatively) interoperable. They have optional schema languages (which can both help and hinder). These formats can be vague about datatypes so you have to be careful with things like numbers or binary strings.
* Binary schema-driven formats like Thrift, Protocol Buffers and Avro allow efficient encoding with clearly defined forward and backward compatibility. The schemas can be useful for documentation. However they need to be decoded before they are human-readable.

These encodings affect their efficiency and the architecture of the apps which use them. In particular, many services need to support rolling upgrades where a new version of a service is gradually deployed to all nodes a few at a time. This allows new versions to be released without downtime and makes deployments less risky, and is beneficial for //evolvability// (the ease of making changes to an application.

During rolling upgrades we must assume that different nodes are running different versions of the application. Thus it is important that all data flowing around is encoded in a way that provides backward compatibility (new code can read old data) and forward compatibility (old code can read new data).

There are several modes of data flow:

* Databases, where the process writing to the DB encodes the data and the process reading from the DB decodes it.
* RPC and REST API calls where the client encodes a request, the server decodes the request and encodes a response, and the client decodes the response.
* Asynchronous message passing using message brokers where nodes communicate by sending each other messages that are encoded by the sending and decoded by the recipient.
