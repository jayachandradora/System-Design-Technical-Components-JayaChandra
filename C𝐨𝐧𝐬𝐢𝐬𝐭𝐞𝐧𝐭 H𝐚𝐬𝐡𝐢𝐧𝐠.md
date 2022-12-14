# Tech-Components-JayaChandra
Tech Components - which provides details of content
![image](https://user-images.githubusercontent.com/115500959/195175318-57663af9-1e03-4c00-a1b9-03996482b431.png)

<br> Algorithms you should know for System Design. 

๐๐ฅ๐ ๐จ๐ซ๐ข๐ญ๐ก๐ฆ 1: ๐๐จ๐ง๐ฌ๐ข๐ฌ๐ญ๐๐ง๐ญ ๐๐๐ฌ๐ก๐ข๐ง๐ <br>

If you prefer text, keep reading:<br>

What do Amazon DynamoDB, Apache Cassandra, Discord, and Akamai CDN have in common?<br>

They all use consistent hashing. Letโs dive right in.<br>

๐๐ก๐๐ญโ๐ฌ ๐ญ๐ก๐ ๐ข๐ฌ๐ฌ๐ฎ๐ ๐ฐ๐ข๐ญ๐ก ๐ฌ๐ข๐ฆ๐ฉ๐ฅ๐ ๐ก๐๐ฌ๐ก๐ข๐ง๐ ?<br>
In a large-scale distributed system, data does not fit on a single server. They are โdistributedโ across many machines. This is called horizontal scaling.<br>

To build such a system with predictable performance, it is important to distribute the data evenly across those servers.<br>

Simple hashing: serverIndex = hash(key) % N, where N is the size of the server pool<br>

This approach works well when the size of the cluster is fixed, and the data distribution is even. But when new servers get added to meet new demand, or when existing servers get removed, it triggers a storm of misses and a lot of objects to be moved. For situations where servers constantly come and go, this design is untenable. <br>

๐๐จ๐ง๐ฌ๐ข๐ฌ๐ญ๐๐ง๐ญ ๐ก๐๐ฌ๐ก๐ข๐ง๐ <br>
Consistent hashing is an effective technique to mitigate this issue.<br>

The goal of consistent hashing is simple. We want almost all objects to stay assigned to the same server even as the number of servers changes.<br>

As shown in the diagram, using a hash function, we hash each server by its name or IP address, and place the server onto the ring. Next, we hash each object by its key with the same hashing function.<br>

To locate the server for a particular object, we go clockwise from the location of the object key on the ring until a server is found. Continue with our example, key 0 is on server 0, key 1 is on server 1. <br>

๐๐๐ ๐ ๐ฌ๐๐ซ๐ฏ๐๐ซ<br>
Now letโs take a look at what happens when we add a server.<br>

Here we insert a new server s4 to the left of s0 on the ring. Note that only k0 needs to be moved from s0 to s4. This is because s4 is the first server k0 encounters by going clockwise from k0โs position on the ring. Keys k1, k2, and k3 are not affected.<br>

๐๐จ๐ฐ ๐๐จ๐ง๐ฌ๐ข๐ฌ๐ญ๐๐ง๐ญ ๐ก๐๐ฌ๐ก๐ข๐ง๐  ๐ข๐ฌ ๐ฎ๐ฌ๐๐ ๐ข๐ง ๐ญ๐ก๐ ๐ซ๐๐๐ฅ ๐ฐ๐จ๐ซ๐ฅ๐<br>
๐นAmazon DynamoDB and Apache Cassandra: minimize data movement during rebalancing<br>
๐นContent delivery networks like Akamai: distribute web contents evenly among the edge servers<br>
๐นLoad balancers like Google Network Load Balancer: distribute persistent connections evenly across backend servers<br>

Over to you - which algorithm should we talk about next?
