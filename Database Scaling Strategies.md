# Database Scaling Strategies

There are basically two types of database scalability:

1. Vertical Scaling or Scale-up
2.  Horizontal Scaling or Scale-out

Scaling has a price, either in terms of cost, performance or usability. Before we dive into the scaling part, here are a few tips on how you can optimize your database performance.

** Proper indexing ** - Indexing is probably the most important part of the query tuning process. An index is a data structure that helps speed up data retrieval. Proper indexing ensures quicker access to the data.

Optimize data access Find out whether your application is retrieving more data than you need. Your queries might be accessing too many rows or too many columns.

Query Optimization (NoSQL)

Avoid SELECT DISTINCT- SELECT DISTINCT is used to remove duplicates from a query. DISTINCT to a SELECT statement can increase the workload unless a unique index exists, one that guarantees that each row will indeed be distinct.
Optimizing INSERT Statements - To optimize insert speed, combine many small operations into a single large operation. Send the data for many new rows at once, and delay all index updates and consistency checking until the very end.
Reduce the use in Select * - Except for the cost of retrieving all the fields, using select * can become a huge problem when modifying code because you do not have control over the fields and their order. As a rule, you should use the actual column names in a SELECT statement instead of the '*'.
Sorting with a mixed order can also be ‘expensive’.
Avoid Correlated SQL Subqueries - A correlated subquery is one that uses values from the parent query. This kind of SQL query tends to run row by row, once for each row returned by the outer query, and thus decreases SQL query performance.
Avoid 'LIKE' searches with wildcards - A wildcard at the beginning of the pattern will prevent the database from using an index for this column’s search.
