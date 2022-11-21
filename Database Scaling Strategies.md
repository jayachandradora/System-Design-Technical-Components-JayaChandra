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
