# Tech-Components-JayaChandra
Tech Components - which provides details of content
![image](https://user-images.githubusercontent.com/115500959/195175318-57663af9-1e03-4c00-a1b9-03996482b431.png)
Algorithms you should know for System Design. 

𝐀𝐥𝐠𝐨𝐫𝐢𝐭𝐡𝐦 1: 𝐂𝐨𝐧𝐬𝐢𝐬𝐭𝐞𝐧𝐭 𝐇𝐚𝐬𝐡𝐢𝐧𝐠<br>

If you prefer video format, watch our YouTube video here ⇩<br>
https://lnkd.in/e6_YMrPt 

If you prefer text, keep reading:<br>

What do Amazon DynamoDB, Apache Cassandra, Discord, and Akamai CDN have in common?<br>

They all use consistent hashing. Let’s dive right in.<br>

𝐖𝐡𝐚𝐭’𝐬 𝐭𝐡𝐞 𝐢𝐬𝐬𝐮𝐞 𝐰𝐢𝐭𝐡 𝐬𝐢𝐦𝐩𝐥𝐞 𝐡𝐚𝐬𝐡𝐢𝐧𝐠?<br>
In a large-scale distributed system, data does not fit on a single server. They are “distributed” across many machines. This is called horizontal scaling.<br>

To build such a system with predictable performance, it is important to distribute the data evenly across those servers.<br>

Simple hashing: serverIndex = hash(key) % N, where N is the size of the server pool<br>

This approach works well when the size of the cluster is fixed, and the data distribution is even. But when new servers get added to meet new demand, or when existing servers get removed, it triggers a storm of misses and a lot of objects to be moved. For situations where servers constantly come and go, this design is untenable. <br>

𝐂𝐨𝐧𝐬𝐢𝐬𝐭𝐞𝐧𝐭 𝐡𝐚𝐬𝐡𝐢𝐧𝐠<br>
Consistent hashing is an effective technique to mitigate this issue.<br>

The goal of consistent hashing is simple. We want almost all objects to stay assigned to the same server even as the number of servers changes.<br>

As shown in the diagram, using a hash function, we hash each server by its name or IP address, and place the server onto the ring. Next, we hash each object by its key with the same hashing function.<br>

To locate the server for a particular object, we go clockwise from the location of the object key on the ring until a server is found. Continue with our example, key 0 is on server 0, key 1 is on server 1. <br>

𝐀𝐝𝐝 𝐚 𝐬𝐞𝐫𝐯𝐞𝐫<br>
Now let’s take a look at what happens when we add a server.<br>

Here we insert a new server s4 to the left of s0 on the ring. Note that only k0 needs to be moved from s0 to s4. This is because s4 is the first server k0 encounters by going clockwise from k0’s position on the ring. Keys k1, k2, and k3 are not affected.<br>

𝐇𝐨𝐰 𝐜𝐨𝐧𝐬𝐢𝐬𝐭𝐞𝐧𝐭 𝐡𝐚𝐬𝐡𝐢𝐧𝐠 𝐢𝐬 𝐮𝐬𝐞𝐝 𝐢𝐧 𝐭𝐡𝐞 𝐫𝐞𝐚𝐥 𝐰𝐨𝐫𝐥𝐝<br>
🔹Amazon DynamoDB and Apache Cassandra: minimize data movement during rebalancing<br>
🔹Content delivery networks like Akamai: distribute web contents evenly among the edge servers<br>
🔹Load balancers like Google Network Load Balancer: distribute persistent connections evenly across backend servers<br>

Over to you - which algorithm should we talk about next?
