# Database Scaling Strategies

There are basically two types of database scalability:

1. Vertical Scaling or Scale-up
2.  Horizontal Scaling or Scale-out

Scaling has a price, either in terms of cost, performance or usability. Before we dive into the scaling part, here are a few tips on how you can optimize your database performance.

**Proper indexing** - Indexing is probably the most important part of the query tuning process. An index is a data structure that helps speed up data retrieval. Proper indexing ensures quicker access to the data.

**Optimize data access** Find out whether your application is retrieving more data than you need. Your queries might be accessing too many rows or too many columns.

**Query Optimization (NoSQL)**

  - **Avoid SELECT DISTINCT- SELECT DISTINCT** is used to remove duplicates from a query. DISTINCT to a SELECT statement can increase the workload unless a unique index exists, one that guarantees that each row will indeed be distinct.
  - **Optimizing INSERT Statements** - To optimize insert speed, combine many small operations into a single large operation. Send the data for many new rows at once, and delay all index updates and consistency checking until the very end.
  - **Reduce the use in Select(*)** - Except for the cost of retrieving all the fields, using select * can become a huge problem when modifying code because you do not have control over the fields and their order. As a rule, you should use the actual column names in a SELECT statement instead of the '*'.
  - **Sorting with a mixed order can also be ‘expensive’.**
  - **Avoid Correlated SQL Subqueries** - A correlated subquery is one that uses values from the parent query. This kind of SQL query tends to run row by row, once for each row returned by the outer query, and thus decreases SQL query performance.
  - **Avoid 'LIKE' searches with wildcards** - A wildcard at the beginning of the pattern will prevent the database from using an index for this column’s search.

**Connection Pool implementation**
Opening a database connection is an expensive operation, connection pooling is used to keep database connections open so they can be reused. With connection pooling, the application server creates a pool of connections to the database, preventing the need to establish a new database connection on each incoming request. Connection pooling has become one of the most common methods of handling database connections before a query request.

![image](https://user-images.githubusercontent.com/115500959/202958425-d94a4fcd-0cdc-4b7a-9bd5-f52dba3041dc.png)

# Scaling your database
  
  ### Vertical Scaling or Scale-up:
  
  The process of adding more physical resources such as memory, storage, and CPU to the existing database server for improving performance.

  - **Pros:** Easier to implement and administer - you only need to handle and manage one system. Application compatibility is retained - from a development standpoint, there’s no need to change anything. The same code will work to the existing database without any issues. Reduced software costs.

  - **Cons:** Single point of failure. Hardware costs can be high. Has a limit - A server can only be so big.

![image](https://user-images.githubusercontent.com/115500959/202959148-f604070f-ca4f-47de-8585-0bc6a7b7f63b.png)

  ### Horizontal scaling
  
  This approach involves adding more instances/nodes of the database to deal with an increased workload. In other words, you add more servers to the cluster when needed. Most database products will not scale in this way, and depending on how this is implemented, applications will need to be re-written to work with the database.
  
  **Apache Casandra**
  Cassandra is a distributed database that runs on a cluster of homogeneous nodes. Cassandra has been architected to handle large volumes of data, providing high availability with no single point of failure. Adding more nodes to Cassandra or reducing the number of nodes is a pretty straightforward task which makes Cassandra an excellent choice for globally distributed datacenters.
  
  ## There are basically three types of Horizontal scaling:
  
  ### 1.  Data replication.
  **Synchronous replication** - synchronous replication products write data to primary storage and the replica simultaneously. As such, the primary copy and the replica should always remain synchronized.

The main benefit of synchronous replication is Consistency – The data on the destination system is consistent and there is no possibility of data loss. But it also comes with a price as Data must be written to both arrays before an I/O operation is done and the Distance between sites impacts performance as latency increases with distance.Synchronous replication also requires low latency (less than 5ms) connection between datacenters.

  **Asynchronous replication** – Data is replicated asynchronously.

The data is copied to the replica after the data is already written to the primary storage. Data synchronization can be triggered by schedule (once per hour/day/week).

Asynchronous replication typically gives a much better response time, and this approach works seamlessly in GEO-redundant architecture when datacenters are located far from each other.

Once again, the improvement in performance comes with a list of cons: Data in remote disk array is never completely synchronized or up to date. The cost of losing data (high RPO) The cost of prolonged recovery time (high RTO)

### Replication Types

1. **Master-Slave Replication**
Enables data from one database server (the master) to be replicated to one or more database servers (the slaves)

It is used to solve performance problems, handle the backup of databases, and as a solution for system failures. This architecture is suitable for scale-out solutions that have a high number of reads and a low number of writes.

Pros: Data consistency - All slaves keep an updated copy of the data as it is on the master, so we can be sure of no data loss in case a master fails. Easy to use- adding a new slave or attaching an existing slave to a new master is relatively easy.

Cons: Increased latency - The master has to wait for an acknowledgment from all of its slaves, this waiting time adds latency.

Reduced Availability: If one of the slave nodes becomes unavailable, then the master blocks all writes and waits until that synchronous replica is available again. In synchronous mode, the failure of any of the replica nodes makes the whole system comes to a standstill.

Write requests can hardly be scaled. The only option to scale writes requests is to scale up the Master node.

When using asynchronous replication, if the master fails then the data will not be available on the slaves.

2. **Multi-Master Replication**
The multi-master technique allows any client to write data to any database server. In a Multi-Master replication scheme, all nodes are equal and can accept both read-only and read-write transactions.

The main problem in the Master-master architecture is that each master needs to perform each and every 'write' and with latency between masters, it is extremely difficult to ensure consistency between masters.

Pros: works seamlessly in GEO-redundant architecture when datacenters are located far from each other. It can be used to protect the availability of a mission-critical database. Multimaster replication is useful for applications that require multiple points of access to database information.

Cons: Masters writing at the same time could lead to conflicts. Ensuring data consistency is challenging because there is no longer a single source of truth. Data in remote disk array is never completely synchronized or up to date. you can’t be sure that backups made on each master node contain the same data.

### 2. External Cache

Caching improves scalability by distributing query workload from the backend to multiple front-end systems, it can dramatically improve both scale and speed.
![image](https://user-images.githubusercontent.com/115500959/202960527-0c35d503-e5e0-498d-8f6d-ea7bac5ed7bd.png)

Redis and Memcached are the top two caching solutions and are widely used across different applications.

#### Caching strategies

  - **Cache-Aside** - best for read-heavy workloads. The cache sits on the side and the application directly talks to both the cache and the database.

  - **Read Through** - work best for read-heavy workloads. The cache sits in between the application and the database. The application only requests data from the cache.

  - **Write-Through** - Data is first written to the cache and then to the database. The cache sits in-line with the database and writes always go through the cache to the main database.

  - **Write Back** - The application still writes data to the cache. However, there is a delay in writing from the cache to the database.

  - **Write-Around** - This technique is similar to write-through cache, but data is written directly to permanent storage, bypassing the cache.

  - **Caching Pros** Performance - reading from cache is extremely fast and it also reduces workloads from the database. Availability - If the database is unavailable, the cache can still be used by the application, making the system more resilient to failures.

  - **Caching Cons** An external cache adds latency. Additional cost. Application complexity. Security overhead. Stale data – a situation where an object in the cache is not the most recent version committed to the data source.

### 3.Sharding (Federated Database )

Sharding your database into multiple servers to improve both read and write performance. This involves distributing reads and write across many nodes. 

When data size grows beyond the overall capacity of a replicated multi-node environment, splitting data becomes unavoidable.

Sharding means distributing data across multiple nodes so each instance contains only a subset of the overall data. For example, you can split your customers between two databases by using the first letter of their last name, ‘A-M’ to db1 and ‘N-Z’ to db2. The problem is that now your application has to deal with two databases.

Each shard must be self-contained because a user transaction can only use data from a single shard. There are several different ways to decide how to break a database into multiple smaller DBs:

  - **Directory-Based Sharding** Done by placing a lookup service in front of the sharded databases. The lookup service knows the current partitioning scheme and keeps a map of each entity and which database shard it is stored on.

  - **Range-based Sharding** Range-based sharding maps the shard key value directly to a server. For example, shard key values 1-100 map to Server #1, shard key values 101-200 map to Server #2

  - **Hash-based Sharding** Hash-based sharding takes the values of our shard key and hashes them in a way that guarantees close to a uniform distribution. This way we can be sure that our data will evenly distribute across shards.

![image](https://user-images.githubusercontent.com/115500959/202960896-7e7ed708-df59-4eba-86a9-2c09a8da58b4.png)

  - **Sharding by Geography** The idea is to create zones of sharded data based on location. It keeps user data close to the user and reduces the distance that the data needs to travel.

  - **Pros** Scalability- In theory, you can add an infinite number of nodes. Resilience and Fault tolerance is generally easier to achieve and manage due to the multiple nodes.

Speeds up the query response times- a database that has not been sharded may have to search every row in the table in order to find the result set you are looking for. By sharding, queries need to go over fewer rows.

  - **Cons** Code is more complex. Backups have to contain data from all the database partitions. Management of the infrastructure becomes more complex with the increased number of nodes. The cost of infrastructure is usually high as latency will lead to inconsistent data. Creating a join query across shards is usually prohibited because of the cost of distributed locking. Sharding is not natively supported by every database engine. For instance, PostgreSQL does not include automatic Sharding.

![image](https://user-images.githubusercontent.com/115500959/202961046-c742b70f-a5ba-40a7-97f3-3ecd86b3512b.png)
