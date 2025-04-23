**Reference** : https://www.linkedin.com/posts/evan-king-40072280_redis-is-the-swiss-army-knife-of-system-design-activity-7320594062607020033-4pG8?utm_source=social_share_send&utm_medium=member_desktop_web&rcm=ACoAACM1duoBiG3PHgy0N4pLCPE98oo0tq_SN8M

Redis is the Swiss Army knife of system design interviews. 
Need a cache? Redis. 
Rate limiter? Redis. 
Priority Queue? Redis again. 
And the list goes on and on.

But whenever you introduce Redis, you should brace for the inevitable question from your interviewer: **"But what if Redis goes down?"**

This isn't just a gotcha question. It targets a genuine vulnerability in many system designs. 
Redis primarily stores data in-memory, which means by default: 
        (1) you have a single point of failure that can bring down critical services, and 
        (2) you risk permanent data loss during crashes or restarts. 
The interviewer is testing whether you've thought beyond basic functionality to consider reliability, availability, and data durability.

Here is how I would answer it:

𝐑𝐞𝐝𝐢𝐬 𝐒𝐞𝐧𝐭𝐢𝐧𝐞𝐥!

Now, of course, those two words alone aren't enough. You'll want to explain what Sentinel is and how it solves the problem.

For this, you can explain :

**Redis Sentinel is a distributed system that monitors Redis instances and automatically handles failover when your primary Redis server fails.**
It's not a feature within Redis itself but a separate set of processes that work alongside Redis to provide high availability.

Here's what you should know:

1. **Distributed monitoring:** Multiple Sentinel processes run on different machines, continuously checking if your Redis primary and replicas are responding

2. **Consensus-based failover:** Sentinels use a quorum system – they vote on whether a primary instance has truly failed before taking action

3. **Automatic promotion:** When failure is confirmed, Sentinels elect a leader that promotes the most up-to-date replica to become the new primary

4. **Client notification:** Sentinel acts as a configuration provider, telling clients where to find the current primary

Importantly, this doesn't solve every problem. For example, if you **need strong consistency, Sentinel isn't right** for you as there is a small window where writes can be lost.

Another critical limitation: **Sentinel only works if your entire dataset fits on a single node.** 
If your data volume exceeds what a single Redis instance can handle, you'll need Redis Cluster instead. 
Redis Cluster provides both sharding (distributing your data across multiple nodes) and high availability, 
whereas Sentinel focuses solely on high availability for a single dataset.

But in the majority of cases, introducing Sentinel to manage replication and monitoring is enough to all but guarantee that you've eliminated any single point of failure 
and achieved high availability in that part of your system.

Additional Read : https://www.hellointerview.com/learn/system-design/deep-dives/redis

