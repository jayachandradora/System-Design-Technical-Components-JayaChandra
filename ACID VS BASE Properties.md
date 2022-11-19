# System-Design-Technical-Components-JayaChandra
Tech Components - which provides details of content

![image](https://user-images.githubusercontent.com/115500959/196360371-8f592704-05f1-4e84-ad8f-599138961d07.png)

# BASE Properties
The rise in popularity of NoSQL databases provided a flexible and fluidity with ease to manipulate data and as a result, a new database model was designed, reflecting these properties. The acronym BASE is slightly more confusing than ACID but however, the words behind it suggest ways in which the BASE model is different and acronym BASE stands for:-

- **Basically Available**: Instead of making it compulsory for immediate consistency, BASE-modelled NoSQL databases will ensure the availability of data by spreading and replicating it across the nodes of the database cluster.
- **Soft State**: Due to the lack of immediate consistency, the data values may change over time. The BASE model breaks off with the concept of a database that obligates its own consistency, delegating that responsibility to developers.
- **Eventually Consistent**: The fact that BASE does not obligates immediate consistency but it does not mean that it never achieves it. However, until it does, the data reads are still possible (even though they might not reflect reality).

Just as SQL databases are almost uniformly ACID compliant, some NoSQL databases also tend to conform to BASE principles like MongoDB, Cassandra and Redis are among the most popular NoSQL solutions, together with Amazon DynamoDB and Couchbase.


https://www.geeksforgeeks.org/acid-model-vs-base-model-for-database/
