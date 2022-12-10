# Tech-Components
Tech Components - which provides details of content

There are hundreds or even thousands of databases available today, such as Oracle, MySQL, MariaDB, SQLite, PostgreSQL, Redis, ClickHouse, MongoDB, S3, Ceph, etc. How do you select the architecture for your system? My short summary is as follows:

🔹Relational database. Almost anything could be solved by them. <br>
🔹In-memory store. Their speed and limited data size make them ideal for fast operations. <br>
🔹Time-series database. Store and manage time-stamped data. <br>
🔹Graph database. It is suitable for complex relationships between unstructured objects. <br>
🔹Document store. They are good for large immutable data. <br>
🔹Wide column store. They are usually used for big data, analytics, reporting, etc., which needs denormalized data. <br>

![image](https://user-images.githubusercontent.com/115500959/206843210-76e0bbd3-96fd-484c-95c0-2a6c3b07b8ed.png)

![image](https://user-images.githubusercontent.com/115500959/195163365-c5840b3e-0710-4600-947f-429fb72e20dd.png)

### Details Here

Choosing the right database is often the most important decision we'll ever make. <br>

We are talking about a database for a real growing business, where a bad choice would lead to extended downtime, customer impact, and even data loss.<br>

This take is probably a bit controversial. The post was written by Sahn Lam and illustrated by me. Follow him (Sahn Lam) for more posts like this.<br>

𝐅𝐢𝐫𝐬𝐭, 𝐚𝐫𝐞 𝐰𝐞 𝐩𝐨𝐬𝐢𝐭𝐢𝐯𝐞 𝐭𝐡𝐚𝐭 𝐰𝐞 𝐧𝐞𝐞𝐝 𝐚 𝐝𝐢𝐟𝐟𝐞𝐫𝐞𝐧𝐭 𝐝𝐚𝐭𝐚𝐛𝐚𝐬𝐞?<br>
Is the existing database breaking at the seams? Maybe the p95 latency is through the roof. 
Maybe the working set is overflowing the available memory, and even the most basic requests need to go to the disk.<br>

𝐖𝐡𝐚𝐭𝐞𝐯𝐞𝐫 𝐭𝐡𝐞 𝐢𝐬𝐬𝐮𝐞𝐬 𝐚𝐫𝐞, 𝐦𝐚𝐤𝐞 𝐬𝐮𝐫𝐞 𝐭𝐡𝐞𝐲 𝐚𝐫𝐞 𝐧𝐨𝐭 𝐞𝐚𝐬𝐢𝐥𝐲 𝐬𝐨𝐥𝐯𝐚𝐛𝐥𝐞.<br>
Let’s read the database manual of our current database system. There could be a configuration knob or two that we can tweak to give us a bit more breathing room.<br>

Can we put a cache in front of it, and give us a few more months of runway?<br>

Can we add read replicas to shed some read load?<br>

Can we shard the database, or partition the data in some way?<br>

The bottom line is this: Migrating live production data is risky and costly. We better be damn sure that there is no way to keep using the current database.<br>

We have exhausted all avenues for the current database. <br>

𝐇𝐨𝐰 𝐝𝐨 𝐰𝐞 𝐠𝐨 𝐚𝐛𝐨𝐮𝐭 𝐜𝐡𝐨𝐨𝐬𝐢𝐧𝐠 𝐭𝐡𝐞 𝐧𝐞𝐱𝐭 𝐨𝐧𝐞?<br>

We developers are naturally drawn to the new and shiny, like moths to flame. When it comes to databases, though, boring is good.<br>

We should prefer the ones that have been around for a long time, and have been battle tested. <br>

𝐒𝐨𝐟𝐭𝐰𝐚𝐫𝐞 𝐞𝐧𝐠𝐢𝐧𝐞𝐞𝐫𝐢𝐧𝐠 𝐚𝐭 𝐬𝐜𝐚𝐥𝐞 𝐢𝐬 𝐚𝐛𝐨𝐮𝐭 𝐭𝐫𝐚𝐝𝐞𝐨𝐟𝐟𝐬. 𝐖𝐡𝐞𝐧 𝐢𝐭 𝐜𝐨𝐦𝐞𝐬 𝐭𝐨 𝐝𝐚𝐭𝐚𝐛𝐚𝐬𝐞𝐬, 𝐢𝐭 𝐢𝐬 𝐞𝐯𝐞𝐧 𝐦𝐨𝐫𝐞 𝐭𝐫𝐮𝐞.<br>

Instead of reading the shiny brochures, go read the manual. There is usually a page called “Limits”. That page is a gem.<br>

Learn as much as possible about the candidate now. The investment is relatively small at this juncture.<br>

𝐎𝐧𝐜𝐞 𝐰𝐞 𝐧𝐚𝐫𝐫𝐨𝐰 𝐝𝐨𝐰𝐧 𝐭𝐡𝐞 𝐝𝐚𝐭𝐚𝐛𝐚𝐬𝐞 𝐨𝐩𝐭𝐢𝐨𝐧𝐬, 𝐰𝐡𝐚𝐭’𝐬 𝐧𝐞𝐱𝐭?<br>
Create a realistic test bench for the candidates using our data, with our real-world access patterns.<br>

During benchmarking, pay attention to the outliers. Measure P99 of everything. The average is not meaningful.<br>

After everything checks out, plan the migration carefully. Write out a detailed step-by-step migration plan.<br>

Picking the right database is not glamorous, and there is a lot of hard work involved. Migrating to a new database in the real world could take years at a high scale.<br>

Good luck.
