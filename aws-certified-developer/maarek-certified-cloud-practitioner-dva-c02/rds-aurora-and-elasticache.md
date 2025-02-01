# RDS, Aurora and ElastiCache

### AWS RDS

* RDS == Relational Database Service.
* It is a managed DB service for DB use SQL as query language.
* It allows you to create databases in the cloud that are managed by AWS.
  * Postgres
  * MySQL
  * MariaDB
  * Oracle
  * Microsoft SQL server
  * Aurora (AWS's DB)

#### Why RDS > DB on EC2?

* RDS is a managed service:
  * **Automated provisioning, OS patching**.
  * Continuous backups and restore to specific timestamp (point in time restore).
  * Monitoring dashboards.
  * **Read replicas** for improved read performance.
  * **Multi AZ setup** for disaster recovery.
  * Maintenance windows for upgrades.
  * Scaling capability (vertical and horizontal).
  * Storage backed by EBS (gp2 or io1).
* BUT you **CANNOT** SSH into **your instances.**

#### **RDS Storage Auto Scaling**

* Helps to dynamically increase storage on the RDS DB instance.
* When RDS detects that the DB is running out of free space, it scales automatically.
* Avoid manually scaling your database storage.
  * :warning: A **maximum storage threshold** must be set.
* Amazon RDS starts a storage modification for an autoscaling-enabled DB instance when these factors apply (AWS, 2023a):
  * Free available space is less than or equal to 10% of the allocated storage.
  * The low-storage condition lasts at least five minutes.
  * At least six hours have passed since the last storage modification, or storage optimization has completed on the instance, whichever is longer.
* Useful for applications with unpredictable workloads.

#### RDS Read Replicas

* Up to 15 Read Replicas
* **Replicas Possible Arrangement:**
  * Within AZ.
  * Cross AZ.
  * Cross Regions.
* Replication is ASYNC :exclamation:
* Replicas can be promoted to their own DB.
* Applications must updated the connection string to leverage read replicas.

<figure><img src="../../.gitbook/assets/image (160).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### RDS Read Replicas - Use Cases

1. You have a production database that is taking on normal load.
2. You want to run a reporting application to run some analytics.
3. You create a Read Replica to run the new workload there.
4. The production application is unaffected.

<figure><img src="../../.gitbook/assets/image (161).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### RDS Read Replicas - Network Cost :money\_with\_wings:

* :exclamation: For RDS Read Replicas within the same region, you don’t pay the fee where data goes from one AZ to another.

<figure><img src="../../.gitbook/assets/image (162).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### RDS Multi AZ (Disaster Recovery)

* The replication is syncronous.
* One DNS name - It helps to automatically failover to the standby version in case of the mater db fails.
* **NOT USED FOR SCALING**

<figure><img src="../../.gitbook/assets/image (163).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* When it is used?
  * Failover in case of loss of AZ.
  * loss of network, instance or storage failure.

#### RDS - From Single AZ to Multi-AZ

* Zero downtime operation (no need to stop the DB.
* Just click on “modify” for the database.
* The following happens internally:
  * A snapshot is taken.
  * A new DB is restored from the snapshot in a new AZ.
  * Synchronization is established between the two databases.

### Amazon Aurora

* **Proprietary Technology:** Aurora is a proprietary technology developed by AWS and is not open sourced.
* **Database Engine Support:** Aurora supports both Postgres and MySQL as Aurora DB, allowing your existing drivers to work seamlessly as if Aurora were a Postgres or MySQL database.
* **Performance:** Aurora is designed to be "AWS cloud-optimized" and offers significant performance improvements compared to MySQL on RDS, with a claimed 5x performance boost, and over 3x the performance of Postgres on RDS.
* **Storage Scalability:** Aurora's storage automatically grows in increments of 10GB, scaling up to an impressive 128TB of data.
* **Replication:** Aurora supports up to 15 read replicas, and its replication process is faster than traditional MySQL, with sub-10 ms replica lag.
* **High Availability (HA):** Aurora is inherently designed for high availability. Failover in Aurora is instantaneous, ensuring minimal downtime in case of primary instance failures.
* **Cost:** While Aurora offers superior performance and high availability, it comes at a higher cost compared to **Amazon RDS**, typically around **20% more expensive**. However, the efficiency and performance improvements may justify the cost for many users.

#### Amazon Aurora High Availability and Read Scaling

**Data Redundancy and Resilience:**

* Aurora ensures high data durability by replicating your data across multiple Availability Zones (AZs). Specifically, :exclamation::exclamation::exclamation:**it maintains six copies of your data across three AZs**.
  * For **write operations**, only :exclamation:**four out of the six copies** are needed. This redundancy ensures data consistency and durability even in the event of AZ failures.
  * For **read operations**, Aurora only requires :exclamation:**three out of the six copies**, allowing for efficient and low-latency read access.
  * Aurora employs self-healing mechanisms with peer-to-peer replication. In the event of data or instance failures, Aurora can automatically recover and maintain data integrity.
* **Instance Roles:**
  * In an Aurora cluster, there is **one primary instance responsible for handling write operations**. This instance ensures data consistency and integrity.
* **Automated Failover:**
  * Aurora provides automated failover capabilities for the primary (master) instance. In the event of a failure, Aurora can initiate a failover process and promote a read replica to the new primary instance in **less than 30 seconds**. This rapid failover minimizes downtime and ensures high availability.
* **Read Scalability:** Master + up to 15 Aurora Read Replicas serve reads.
* **Cross-Region Replication:**
  * Aurora offers support for Cross Region Replication, allowing you to replicate your Aurora database to a different AWS region for disaster recovery, data locality, or other purposes. This feature enhances the database's resilience and availability.

<figure><img src="../../.gitbook/assets/image (164).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Aurora DB Cluster

<figure><img src="../../.gitbook/assets/image (165).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

> Now that you have auto-scaling, it can be difficult for your application to keep track of where are your read replicas. What is the URL for the newest read replicas. How do I connect to them?
>
> To solve these problems there is the **Reader Endpoint. A Reader Endpoint has exactly the same feature as a Writer Endpoint. It helps with connection load balancing by automatically connecting to all the Read Replicas.** So every time the client connects to the reader endpoints, it will get connected to one of the read replicas and there will be load balancing done this way. Note that the load balancing happens at the connection level, not the statement level.
>
> by MAAREK, 2023.

<figure><img src="../../.gitbook/assets/image (166).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (167).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Aurora's Features

• Automatic fail-over • Backup and Recovery • Isolation and security • Industry compliance • Push-button scaling • Automated Patching with Zero Downtime • Advanced Monitoring • Routine Maintenance • Backtrack: restore data at any point of time without using backups.

### RDS & Aurora Security

* At-rest encryption:
  * Database master & replicas encryption using AWS KMS – must be defined as launch time.
  * If the master is not encrypted, the read replicas cannot be encrypted.
  * To encrypt an un-encrypted database, go through a DB snapshot & restore as encrypted.
* In-flight encryption: TLS-ready by default, use the AWS TLS root certificates client-side.
* IAM Authentication: IAM roles to connect to your database (instead of username/pw).
* Security Groups: Control Network access to your RDS / Aurora DB.
* No SSH available except on RDS Custom.
* Audit Logs can be enabled and sent to CloudWatch Logs for longer retention.

### Amazon RDS Proxy

* It is a fully managed database proxy for RDS.
* Allows apps to pool and share DB connections established with the database.
* :exclamation:**Improving database efficiency by reducing the stress on database resources (e.g., CPU, RAM) and minimize open connections (and timeouts).**
* :exclamation:**Reduced RDS & Aurora failover time by up 66%.**

<details>

<summary>More about "<strong>Reduced RDS &#x26; Aurora failover time by up 66%".</strong></summary>

1. **Connecting to the Main RDS Database Instance:**
   * In a typical RDS setup, applications connect directly to the main RDS database instance. This direct connection means that the applications are aware of the specific database instance they are connecting to.
2. **Handling Failover Themselves:**
   * When a failover event occurs (for example, due to a hardware issue or maintenance), it's the responsibility of the applications to detect the failure and switch to a standby or secondary database instance.
   * This failover process can be complex and requires the applications to handle reconnection logic, DNS updates, and other tasks to ensure they continue operating smoothly during and after the failover.
3. **RDS Proxy Simplifies Failover Handling:**
   * With the introduction of an RDS Proxy, applications no longer need to be aware of the failover process or handle it themselves.
   * Instead of connecting directly to the main RDS database instance, applications connect to the RDS Proxy. The RDS Proxy acts as an intermediary between the applications and the database.
   * Importantly, the RDS Proxy is not aware of the failover process; it simply routes incoming database connections to the main RDS database instance.
4. **RDS Proxy Handles Failover Automatically:**
   * When a failover event occurs, such as the main RDS instance becoming unavailable, the RDS Proxy is designed to automatically detect the failure.
   * The RDS Proxy then initiates the failover process on behalf of the applications. It seamlessly redirects incoming database connections to a standby or secondary RDS instance that has been promoted to the new primary.
   * This automation significantly improves the failover time because it eliminates the need for applications to detect and react to failures, which can introduce delays and potential errors.

</details>

<details>

<summary>But what about the feature "One DNS name" of RDS? Doesn't it simplify the configuration to reconnect to the RDS DB?</summary>

Yes, the "One DNS name" feature of Amazon RDS can simplify the configuration for applications to reconnect to the RDS database during failover events, especially in Multi-AZ deployments. Here's how it works:

In a Multi-AZ RDS deployment, you are provided with a DNS endpoint for your database instance. This DNS endpoint represents the current primary database instance, but it automatically redirects connections to the appropriate instance in the event of a failover. This redirection is handled transparently, and your applications do not need to be aware of the underlying failover process.

Here's how this simplifies things:

1. **Single DNS Endpoint:** You configure your applications to connect to the single DNS endpoint provided by RDS for your database instance.
2. **Automatic Failover:** In the event of a failover (e.g., due to an AZ outage or maintenance), RDS automatically promotes the standby instance to become the new primary.
3. **Transparent Connection Redirection:** During and after the failover, RDS updates the DNS endpoint to point to the new primary instance. Your applications, which are configured to use this DNS endpoint, automatically start connecting to the new primary without requiring any changes or manual intervention.

_This "One DNS name" feature abstracts the underlying complexity of failover and makes it easier to configure and manage your applications. It ensures that your applications continue to work seamlessly even when failover events occur, and it reduces the need for manual reconfiguration or updates._

_While this feature simplifies the configuration to reconnect to the RDS database, using an RDS Proxy can still offer additional benefits, such as connection pooling and load balancing, which can further improve the performance and scalability of your database interactions. However, for many use cases, relying on the "One DNS name" feature alone is sufficient for handling failover gracefully._

</details>

* **It is a service** that is Serverless, can autoscale and is highly available (multi-AZ).
* Supports RDS (MySQL, PostgreSQL, MariaDB, MS SQL Server) and Aurora (MySQL, PostgreSQL).
* No code changes required for most apps.
* Enforce IAM Authentication for DB, and securely store credentials in AWS Secrets Manager.
  * > It helps asserting that people/applications only connect to the DB via the IAM/proxy.
* RDS Proxy is never publicly accessible (must be accessed from within the VPC).

#### Lambda and RDS Proxy

Lambda functions can scale rapidly, creating and terminating connections very quickly. **This rapid scaling can lead to issues such as connections left opened by terminated lambdas and timeouts when connecting to the RDS database.**

To address this problem, RDS Proxy can be used. The RDS Proxy acts as an intermediary layer between Lambda functions and the RDS database. It efficiently manages and pools database connections, handling the rapid influx and termination of connections from Lambda functions.

:exclamation:**By using the RDS Proxy, Lambda functions can continue to scale and create connections without overwhelming the RDS database instance.** The RDS Proxy optimizes the connection pooling process, reducing the number of connections required between the Lambda functions and the database.

### Amazon ElastiCache

* The same way RDS is to get managed relational Databases.
* ElastiCache is to get managed Redis or Memcached.
* Caches are in-memory databases with really high performance, low latency.
* Helps reduce load off of databases for read intensive workloads.
* Helps make your application stateless.
* Note: Using ElastiCache involves heavy application code changes.
* The maximum number of Read Replicas you can add in an ElastiCache Redis Cluster with Cluster-Mode Disabled is 5.

#### ElastiCache Solution Architecture - DB Cache

* Applications queries ElastiCache, if not available, get from RDS and store in ElastiCache.
* Helps relieve load in RDS.
* Cache must have an invalidation strategy to make sure only the most current data is used in there.

<figure><img src="../../.gitbook/assets/image (168).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### ElastiCache Solution Architecture - User Session Store

* User logs into any of the application.
* The application writes the session data into ElastiCache.
* The user hits another instance of our application.
* The instance retrieves the data and the user is already logged in.
* Note: By doing this, your application became stateless.

<figure><img src="../../.gitbook/assets/image (169).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### ElastiCache Redis vs. Memcached (MAAREK, 2023)

| Redis                                                                      | Memcached                           |
| -------------------------------------------------------------------------- | ----------------------------------- |
| Multi AZ with Auto-Failover.                                               | Multi-node for par.                 |
| Read Replicas to scale reads and have **high availability (replication)**. | No high availability (replication). |
| **Data durability** using AOF persistence.                                 | Non persistent.                     |
| Backup and restore features.                                               | No backup and restore.              |
| Supports Sets and Sorted Sets                                              | multi-threaded architecture.        |

### Caching Implementation

#### Caching Considerations

* Is it safe to cache data? Data may be out of date, eventually consistent.
* **Is caching effective for that data?**
  * Pattern: data changing slowly, few keys are frequently needed.
  * Anti-pattern: data changing rapidly, all large key space frequently needed.
* **Is data structured well for caching?**
  * example: key value caching, or caching of aggregations result.
* Cache Patterns:
  * Lazy Loading/ Cache-Aside/ Lazy Population.
  * Write Through.

#### Cache Pattern: Lazy Loading (or Cache- Aside, Lazy Population)

| Pros                                                                         | Cons                                                                                 |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Only requested data is cached (the cache is not filled up with unused data). | Cache miss penalty that results in 3 roud trips (noticeable delay for that request). |
| Node failures are not fatal (just increased latency to warm the cache).      | Stale data: Data can be updated in the database and outdatd in the cache.            |

<figure><img src="../../.gitbook/assets/image (170).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Cache Pattern: Write Through

* Add or Update cache when database is updated.

| Pros                                           | Cons                                                                                                         |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| Data in cache is never stale, reads are quick. | Write penalty (each write requires 2 calls).                                                                 |
|                                                | Missing data until it is added/updated in the DB (mitigation is to implement lazy loading strategy as well). |

<figure><img src="../../.gitbook/assets/image (171).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Cache Evictions and Time-to-live (TTL)

* Cache eviction can occur in three ways:
  * You delete the item explicitly in the cache.
  * Item is evicted because the memory is full and it’s not recently used (LRU).
  * You set an item time-to-live (or TTL).
* If too many evictions happen due to memory, you should scale up or out.

Question: You have an ElastiCache Redis Cluster that serves a popular application. You have noticed that there are a large number of requests that go to the database because a large number of items are removed from the cache before they expire. What is this called and how to solve it?

<details>

<summary>Answer</summary>

Cache Evictions, Scale up or out your elasticCache redis Cluster

</details>

### Amazon MemoryDB for Redis.

* Redis-compatible, **durable**, in-memory database service.
* The main difference between Redis and MemoryDB for Redis, is that while Redis is intent to be used as a cache DB with some durability, **MemoryDB is really a database** that has a Redis compatible API\*\*.\*\*
* **It is in-memory DB, but it's durable data storage with Multi-AZ transaction log.**
  * "Multi-AZ transaction log" refers to the mechanism MemoryDB uses to replicate changes made to the data across these Availability Zones. The transaction log records and replicates all data modifications, ensuring data consistency and minimizing the risk of data loss in the event of an AZ failure.
  * See more about what is transaction logs in this note: [Database Concepts](https://app.gitbook.com/s/uBtdKjBB6wlw1AXXdtRP/ "mention")

While Amazon ElastiCache for Redis is a managed version of Redis, an in-memory data store used mainly for caching,Amazon MemoryDB for Redis aims to replace both cache and database in one component.

MemoryDB also supports multi-AZ availability with up to 5 replicas in different AZs.

It offers strong consistency on the primary node, eventual consistency reads on replica nodes.

Use case: If your workload requires a durable database that provides ultra-fast performance, you should consider using Amazon MemoryDB for Redis.

<figure><img src="../../.gitbook/assets/image (174).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### Reference

* AWS. _Working with storage for Amazon RDS DB instances_. Amazon Web Services. Disponível em: [https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER\_PIOPS.StorageTypes.html](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_PIOPS.StorageTypes.html#USER_PIOPS.Autoscaling). Acesso em: 11 set. 2023a.
