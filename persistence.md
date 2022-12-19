# Choosing the Database

## Introduction

Relational and NoSQL are two types of database systems commonly implemented in cloud-native applications. They are compiled differently, store data differently, and accessed differently. In this section, we'll look at both. Later in this chapter, we'll look at an emerging database technology called NewSQL.

Relational databases have been a mainstream technology for decades. They are mature, proven and widely implemented. Competing database products, tools, and experience abound. Relational databases provide a repository of related data tables. These tables have a fixed schema, use SQL (SQL language) to manage data, and support ACID guarantees.

Non-SQL Databases refer to high performance non-relational data stores. They excel in their ease of use, scalability, resiliency, and availability characteristics. Rather than joining tables of normalized data, NoSQL stores unstructured or semi-structured data, often in key-value pairs or JSON documents. Non-SQL databases typically do not provide ACID guarantees beyond the scope of a single database partition. High-volume services that require sub-second response time favor NoSQL datastores.

NoSQL databases include several different models for accessing and managing data, each suited to specific use cases. Figure below shows four common models.

<img src="https://learn.microsoft.com/pt-br/dotnet/architecture/cloud-native/media/types-of-nosql-datastores.png"
     alt="CAP's theorem"
     style="float: left; margin-right: 10px;" />

## The CAP's Theorem

As a way to understand the differences between these types of databases, consider the CAP theorem, a set of principles applied to distributed systems that store state. Figure below shows the three properties of the CAP theorem.

<img src="https://learn.microsoft.com/pt-br/dotnet/architecture/cloud-native/media/cap-theorem.png"
     alt="CAP's theorem"
     style="float: left; margin-right: 10px;" />

The theorem states that distributed data systems will offer a tradeoff between consistency, availability, and partition tolerance. And that any database can only guarantee two of the three properties:

- Consistency. Every node in the cluster responds with the latest data, even if the system needs to block the request until all replicas are updated. If you query a "system consistent" for a currently updated item, it will wait for that response until all replicas are successfully updated. However, you will receive the most current data.

- Availability. Each node returns an immediate response, even if that response is not the most recent data. If you query a "system available" for an item you are updating, you will get the best possible answer the service can provide at that time.

- Partition Tolerance. Ensures that the system continues to operate even if a replicated data node fails or loses connectivity to other replicated data nodes.

The CAP theorem explains the tradeoffs associated with managing consistency and availability during a network partition; however, consistency and performance tradeoffs also exist with the absence of a network partition. CAP's theorem is usually extended further to PACELC to explain trade-offs more comprehensively.

Relational databases typically provide consistency and availability, but not partition tolerance. These are typically provisioned on a single server and scaled up by adding more resources to the computer.

Many relational database systems support built-in replication capabilities where copies of the primary database can be made to other secondary server instances. Write operations are done on the primary instance and replicated to each of the secondary ones. After a failure, the primary instance can fail over to a secondary to provide high availability. Children can also be used to distribute read operations. While write operations always go against the primary replica, read operations can be routed to any of the secondaries to reduce system load.

Data can also be partitioned horizontally across multiple nodes, as with sharding. However, fragmentation dramatically increases operational overhead by splitting data into many pieces that cannot easily communicate. It can be expensive and time consuming to manage. Relational features including table joins, transactions, and referential integrity incur steep performance penalties in fragmented deployments.

Replication consistency and recovery point objectives can be tuned by configuring whether replication occurs synchronously or asynchronously. If data replicas lose network connectivity in a “highly consistent” or synchronous relational database cluster, you will not be able to write to the database. The system would reject the write operation as it cannot replicate this change to the other data replica. Each data replica needs to be updated before the transaction can complete.

NoSQL databases typically support high availability and partition tolerance. They scale horizontally, often across commodity servers. This approach provides tremendous availability, within and across geographic regions, at a reduced cost. You partition and replicate data across these computers or nodes, providing redundancy and fault tolerance. Consistency is typically adjusted through consensus protocols or quorum mechanisms. They provide more control when navigating the tradeoffs between synchronous versus asynchronous replication tuning in relational systems.

If data replicas lose connectivity on a "highly available" NoSQL DB cluster, you can still complete a write operation to the database. The DB cluster would allow the write operation and update each data replica as it becomes available. NoSQL databases that support multiple writable replicas can further strengthen high availability by avoiding the need for failover while optimizing the recovery time objective.

Modern NoSQL databases typically implement partitioning features as a system design feature. Partition management is typically internal to the database and routing is accomplished through placement hints, often called partition keys. A flexible data model enables NoSQL databases to reduce the burden of schema management and improve availability when deploying application updates that require changes to the data model.

High availability and massive scalability are often more business critical than relational table joins and referential integrity. Developers can implement techniques and patterns like Sagas, CQRS, and asynchronous messaging to embrace eventual consistency.

> Nowadays, care must be taken when considering the constraints of the CAP theorem. A new type of database, called NewSQL, has emerged that extends the relational database engine to support the horizontal scalability and scalable performance of NoSQL systems.

## SQL versus NoSQL

### Consider a NoSQL datastore when

- You have high-volume workloads that require predictable latency at scale (for example, latency measured in milliseconds when running millions of transactions per second)

- Your data is dynamic and changes frequently

- Relations can be denormalized data models

- Data recovery is simple and express without table joins

- Data is typically replicated across geographies and requires finer control over consistency, availability, and performance

- Your application will be deployed on commodity hardware such as with public clouds

### Consider a relational database when

- Your workload volume usually fits in thousands of transactions per second

- Your data is highly structured and requires referential integrity

- Relationships are expressed through table joins in normalized data models

- You work with complex queries and reports

- Data is typically centralized or can be replicated asynchronously

- Your application will be deployed on large, high-end hardware

| Feature | NoSQL | SQL |
|---------|-------|-----|
| Performance | faster | ${\color{red}Slower}$ |
| Reliability | worst | better |
| Availability | good | good |
| Consistency | worst | better |
| Data Storage | Optimized for huge data | Medium size to large |
| Scalability | High | High (but more expensive) |

As much as the project at the current level supports a SQL database, analyzing these differences the team opted for a non-relational database as an opportunity to learn more about NoSQL and to allow better horizontal scalability if needed in the future.
