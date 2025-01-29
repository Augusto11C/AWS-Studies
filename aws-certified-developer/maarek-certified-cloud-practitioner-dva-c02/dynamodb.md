# DynamoDB

* Fully managed, highly available with replication across multiple AZs.
* NoSQL database - not a relational database.
* Scales to massive workloads, distributed database.
* Millions of requests per seconds, trillions of row, 100s of TB of storage.
* Fast and consistent in performance (low latency on retrieval).
* Integrated with IAM for security, authorization and administration.
* Enables event driven programming with DynamoDB Streams.
* Low cost and auto-scaling capabilities.
* Standard & Infrequent Access (IA) Table Class.

***

* DynamoDB is made of Tables.
* Each table has a **Primary Key** (must be decided at creation time).
* Each table can have an infinite number of **items** (= **rows**).
* Each item has **attributes** (can be added over time – can be null).
* Maximum size of an item is 400KB.
* Data types supported are:
  * **Scalar Types** - String, Number, Binary, Boolean, Null.
  * **Document Types** - List, Map.
  * **Set Types** - String Set, Number Set, Binary Set.

### DynamoDB - Primary Keys

#### Partition Key (HASH)

* Partition key must be unique for each item.
* Partition key must be “diverse” so that the data is distributed.

<figure><img src="../../.gitbook/assets/image (296).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Partition Key + Sort Key (HASH + RANGE)

* The combination must be unique for each item.
* Data is grouped by partition key.
* Example: users-games table, “User\_ID” for Partition Key and “Game\_ID” for Sort Key.

<figure><img src="../../.gitbook/assets/image (297).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### DynamoDB - Read/Write Capacity Modes

Note: You can switch between different modes once every 24 hours.

#### Provisioned Mode (default)

* You specify the number of reads/writes per second.
* You need to plan capacity beforehand.
* Pay for **provisioned** read & write capacity.
* Read Capacity Units (RCU) – throughput for reads.
* Write Capacity Units (WCU) – throughput for writes.
* Option to setup **auto-scaling** of throughput to **meet demand**.
* Throughput can be exceeded temporarily using “**Burst Capacity**”.
  * If Burst Capacity has been consumed, you’ll get a `ProvisionedThroughputExceededException`.
  * It’s then advised to do an exponential backoff retry.

**Console - Provisioned Mode With Auto Scaling Enabled**

<figure><img src="../../.gitbook/assets/image (293).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

**Console - Provisioned Mode With Auto Scaling Disabled**

<figure><img src="../../.gitbook/assets/image (295).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### On-Demand Mode

* Read/writes automatically scale up/down with your workloads.
* No capacity planning needed.
* Pay for what you use, more expensive (\$$$).
* Read/writes automatically scale up/down with your workloads.
* No capacity planning needed (WCU / RCU).
* Unlimited WCU & RCU, no throttle, more expensive.
* You’re charged for reads/writes that you use in terms of RRU and WRU.
* **Read Request Units (RRU)** - throughput for reads (same as RCU).
* **Write Request Units (WRU)** - throughput for writes (same as WCU).
* **2.5x more expensive than provisioned capacity (use with care).**
* Use cases: unknown workloads, unpredictable application traffic.

### DynamoDB - Write Capacity Units (WCU)

* One Write Capacity Unit (WCU) represents one write per second for an item up to 1 KB in size.
* If the items are larger than 1 KB, more WCUs are consumed.

<figure><img src="../../.gitbook/assets/image (290).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### DynamoDB Strongly Consistent Read vs. Eventually Consistent Read.

* Eventually Consistent Read (default):
  * If we read just after a write, it’s possible we’ll get some stale data because of replication.
* Strongly Consistent Read:
  * If we read just after a write, we will get the correct data.
  * Set “ConsistentRead” parameter to True in API calls (GetItem, BatchGetItem, Query, Scan).
  * Consumes twice the RCU.

### DynamoDB - Read Capacity Units (RCUs)

* One Read Capacity Unit (RCU) represents one Strongly Consistent Read (SCR) per second, or two Eventually Consistent Reads (ECR) per second, for an item up to 4 KB in size.
* If the items are larger than 4 KB, more RCUs are consumed.

<figure><img src="../../.gitbook/assets/image (292).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* Calculation involving Eventually Consistent Reads: ECR/2 \* ItemSize/4KB.
* Calculation involving Strongly Consistent Read: SCR \* ItemSize/4KB

### DynamoDB - Partitions Internal

* Data is stored in partitions.
* Partitions keys go through a hashing algorithm to know to which partition they go to.
* In DynamoDB, data is organized into tables, which are further divided into partitions. Partitions are the fundamental unit of scalability in DynamoDB. Each partition is stored on its own server to ensure high availability and fault tolerance. The number of partitions in a DynamoDB table is initially determined based on the table's provisioned throughput.
* WCUs and RCUs are spread evenly across partitions.
* :warning: :warning: :warning: :warning: :warning: :warning: If you have 10 partitions, and you provide a new provision with 10 **WCUs** and 10 **RCUs**, then they're going to be spread evenly across partitions.
  * The even distribution of capacity ensures that the workload is spread across the partitions, preventing hotspots where certain partitions become overloaded with requests while others remain underutilized. DynamoDB's automatic partition management takes care of distributing the data and load balancing the requests to provide optimal performance and scalability.

<figure><img src="../../.gitbook/assets/image (291).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### DynamoDB - Throttling

* If we exceed provisioned RCUs or WCUs, we get "ProvisionedThroughputExceeded Exception.
* **Reasons:**
  * Hot Keys (e.g., popular item).
  * Hot partitions.
  * Very Large items.
* **Solutions:**
  * Exponential Backoff
  * Distribute Partition Keys as much as possible
  * if RCU issue, we can youse DynamoDB Accelarator (DAX).

### DynamoDB - Writing Data

* PutItem:
  * Creates a new item or fully replace an old item (same Primary Key).
  * Consumes WCUs.
* UpdateItem:
  * Edits an existing item’s attributes or adds a new item if it doesn’t exist.
  * Can be used to implement **Atomic Counters** – a numeric attribute that’s unconditionally incremented.
* Conditional Writes:
  * Accept a write/update/delete only if conditions are met, otherwise returns an error.
  * Helps with concurrent access to items.

### DynamoDB - Reading Data

* GetItem
  * Read based on primary key.
  * Primary key can be HASH or HASH+RANGE.
  * Eventually Consistent Read (default).
  * Option to use Strongly Consistent Reads (more RCU - might take longer).
  * ProjectionExpression can be specified to retrieve only certain attributes.

### DynamoDB - Reading Data (Query)

* Query returns items based on:
  * KeyConditionsExpressions
    * Partition Key Value (must be = operator) - required.
    * Sort Key value (=, <, <=, >, >=, Between, Begins with) - optional.
* Returns:
  * The number of items specified in **limit**.
  * **or up to 1 MB of data**.
* Ability to do pagination on the results.
* Can query table, a local secondary index, or a Global Secondary Index.

### DynamoDB - Reading Data (Scan)

* **Scan the entire table** and then filter out data (inefficient).
* Returns up to I MB of data - use pagination to keep on reading.
* :warning: Consumes a lot of RCU.
* Limit impact using Limit or reduce the size of the result and pause.
* For faster performance, use Parallel Scan.
  * Multiple workers scan multiple data segments at the same time.
  * Increases the throughput and RCU consumed.
  * Limit the impact of parallel scans just like you would for Scans.
* Can use ProjectionExpression & FilterExpression ( :warning: **no changes to RCU**).

### DynamoDB - Deleting Data (Scan)

* DeleteItem
  * Delete an Individual item.
  * ability to perform a conditional delete.
* DeleteTable
  * Delete a whole table and all its items.
  * Much quicker deletion than calling DeleteItem on all Items.

### DynamoDB - Batch Operations (Scan)

* Allows you to **save in latency by reducing the number of API calls**.
* Operations are done in parallel for better efficiency.
* Part of a batch can fail; in which case we need to try again for the failed items.
* **BatchWriteItem**
  * Up to 25 PutItem and/or DeleteItem in one call.
  * Up to 16 MB of data written, up to 400KB of data per item.
  * Can't update iteams (use UpdateItem).
  * :warning: **UnprocessedItems** for failed write operations (exponential backoff or add WCU).
    * If there are items that couldn't be written, typically due to insufficient write capacity, you'll receive something known as UnprocessedItems. **You can then retry writing the items contained within UnprocessedItems.**
    * :warning:If you're consistently encountering UnprocessedItems and experiencing scaling issues, **it's advisable to increase your write capacity units.** This will enhance the efficiency of your batch operations.
* **BatchGetItem**
  * Return items from one or more tables.
  * Up to 100 items, up to 16 MB of data.
  * Items are retrieved in parallel to minimize latency.
  * UnprocessedKeys for failed read operations (exponential backoff or add RCU).

### DynamoDB - PartiQL

PartiQL is a SQL-compatible query language for DynamoDB that allows one to perform operations such as select, insert, update, and delete data. It enables the running of queries across multiple DynamoDB tables. One can execute PartiQL queries from various platforms including the AWS Management Console, NoSQL Workbench for DynamoDB, DynamoDB APIs, AWS CLI, and AWS SDK.

### DynamoDB - Conditional Writes

* For PutItem, UpdateItem, DeleteItem, and BatchWriteItem.
* You can specify a Condition expression to determine which items should be modified:
  * attribute\_exists
  * attribute\_not\_exists
  * attribute\_type
  * contains (for string)
  * begins\_with (for string)
  * `ProductCategory IN (:cat1, :cat2) and Price between :low and :high`
  * size (string length)

:warning:Note: **Filter Expression** filters the results of **read queries**, while **Condition Expressions** are for **write operations.**

**Example: Update Item**

<figure><img src="../../.gitbook/assets/image (298).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

**Example: Delete Item**

<figure><img src="../../.gitbook/assets/image (299).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

**Example: String Comparisons**

<figure><img src="../../.gitbook/assets/image (300).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Do Not Overwrite Elements While Performing Writing Operation

* `attribute_not_exists(partition_key)`: Make sure the item isn’t overwritten.
* `attribute_not_exists(partition_key) and attribute_not_exists(sort_key)` :
  * Make sure the partition / sort key combination is not overwritten.

### DynamoDB - Global Secondary Index (GSI)

* An alternative Primary Key (HASH or HASH+RANGE) is derived from the base table.
* Queries on non-key attributes are accelerated.
* The Index Key is composed of scalar attributes (String, Number, or Binary).
* Attribute Projections can include some or all the attributes of the base table (KEYS\_ONLY, INCLUDE, ALL).
* RCUs & WCUs must be provisioned for the index.
* The index can be added or modified after the table has been created.

#### Use Case Example (Be a Better Dev, 2019b)

*   Given the table below: \*

    ```
    <figure><img src="../../.gitbook/assets/image (138).png" alt=""><figcaption><p>Font: Be a Better Dev, 2019b</p></figcaption></figure>
    ```

    * Give me all the rows where: `OriginCountry == "Germany"`.
    * **Not Ideal Approach:**
      * Scan + FilterExpression("OriginCountry == "Germany").
      * This approach is going to check each one of the rows in this DynamoDB table and it will verify if the if OriginCountry == "Germany".
      * This approach is not scalable when your table has millions of rows.
    * **GSI Approach:**
      * Make a GSI of OriginCountry.
      * Feach "Germany" with one lookup.

{% code overflow="wrap" %}
```java
DynamoDBQueryExpression<Account> q = new DynamoDBQueryExpression<Account>()
.withHashKeyValues(account)
.withIndexName("OriginCountry")
.withLimit(resultLimit);
```
{% endcode %}

#### How Does GSI Work?

* You as a user define a GSI Index - This involves selecting a new Partition Key.
* :warning: :warning: Creating a GSI **CLONES** your primary table using your new Partition Key, but keeps these two tables in sync.

<figure><img src="../../.gitbook/assets/image (306).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (308).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (310).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (311).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (307).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (302).png" alt=""><figcaption><p>Font: Be a Better Dev, 2019b</p></figcaption></figure>

* GSI Partition Key Requires Uniform Data Distribution.
* :warning::warning: Define RCU/WCU Capacity separately on the index.
* Throttling.

#### Main Points About GSI to Keep in Mind (Be a Better Dev, 2019b)

* Writes to the main table result in a write to the GSI, effectively doubling the cost of writing.
* Writes to the main table are eventually replicated on the GSI (usually very quickly, but no guaranteed SLA).
* Race conditions due to eventual consistency - your GSI can potentially return stale data.
* Keep your WCU capacoty on your GSI tables >= the WCU capacity on your main table.

### DynamoDB - Local Secondary Index (LSI)

* An alternative Sort Key for the table is provided, which shares the same Partition Key as the base table.
* The Sort Key is composed of a single scalar attribute (String, Number, or Binary).
* A maximum of 5 Local Secondary Indexes per table is allowed.
* The definition of these indexes must occur at the time of table creation.
* Attribute Projections can include some or all the attributes of the base table (KEYS\_ONLY, INCLUDE, ALL).

### DynamoDB - Optimistic Locking

DynamoDB's Optimistic Locking is a strategy that ensures the item you are updating (or deleting) is the same as the item in Amazon DynamoDB. This strategy protects your database writes from being overwritten by the writes of others, and vice versa¹.

Here's how it works:

* Each item has an attribute that acts as a version number.
* When you retrieve an item from a table, the application records the version number of that item.
* You can update the item, but only if the version number on the server side has not changed.
* If there is a version mismatch, it means that someone else has modified the item before you did.
* The update attempt fails because you have a stale version of the item.
* If this happens, you simply try again by retrieving the item and then trying to update it.

***

* Accidental overwriting of changes made by others is prevented by optimistic locking, and it also safeguards against others inadvertently overwriting your changes.
* The @DynamoDBVersionAttribute annotation, provided by the AWS SDK for Java, supports optimistic locking.
* One property is designated in the mapping class for your table to store the version number, and this property is marked using this annotation.
* An attribute that stores the version number will be present in the corresponding item in the DynamoDB table when an object is saved.
* A version number is assigned by the DynamoDBMapper when you first save the object, and this version number is automatically incremented each time the item is updated.
* Only when the client-side object version matches the corresponding version number of the item in the DynamoDB table do your update or delete requests succeed.
* A ConditionalCheckFailedException is thrown if there’s a mismatch.

<figure><img src="../../.gitbook/assets/image (312).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### DynamoDB Accelerator (DAX)

* DynamoDB Accelerator (DAX) is a fully-managed, highly available, seamless in-memory cache for DynamoDB.
* It offers microseconds latency for cached reads and queries.
* There’s no need for application logic modification as it’s compatible with existing DynamoDB APIs.
* It addresses the “Hot Key” problem, which occurs when there are too many reads.
  * :warning::warning::warning:**The hot key problems are addressed by DAX. Throttling on your RCU's may occur if a very specific key or item is read too many times. However, this problem is mitigated if the item is cached by DAX.**
*   The Time to Live (TTL) for cache is 5 minutes by default.

    * Custom TTL -> Parameter Group.
    *

    ```
    <figure><img src="../../.gitbook/assets/image (153).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>
    ```

    *

    ```
    <figure><img src="../../.gitbook/assets/image (154).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>
    ```

    *

    ```
    <figure><img src="../../.gitbook/assets/image (155).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>
    ```
* The cluster can have up to 10 nodes.
* For production, a minimum of 3 nodes is recommended for Multi-AZ deployment.
* It ensures security with features like Encryption at rest with KMS, VPC, IAM, and CloudTrail.

<figure><img src="../../.gitbook/assets/image (313).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (315).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### DAX vs. ElastiCache

DAX is a caching service provided by Amazon for DynamoDB, a NoSQL database. When using DAX, you can create a cache for individual objects or for your queries or scans. This caching feature is very useful for simple types of queries where you can store the results and avoid the need to perform the same computation every time.

However, in more complex scenarios where you have logic applications, such as performing scans, calculating sums, and filtering data, repeatedly executing these operations can be computationally expensive. To optimize performance, you can leverage Amazon ElastiCache.

ElastiCache is an in-memory data store provided by Amazon that supports popular caching engines such as Redis or Memcached. Instead of re-querying DAX and re-performing the aggregation on the client side, you can store the results of your application's operations in ElastiCache. This means that you can retrieve the data directly from ElastiCache, avoiding the need for repetitive and costly computations.

In summary, combining DAX and ElastiCache in your architecture allows you to benefit from DAX's caching capabilities for simple queries and leverage ElastiCache for more complex logic applications, reducing computational expenses and improving performance.

<figure><img src="../../.gitbook/assets/image (314).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### DynamoDB Stream

* An ordered stream of item-level modifications (create/update/delete) in a table is provided.
* Stream records have the capability to be:
  * Dispatched to Kinesis Data Streams.
  * Accessed by AWS Lambda.
  * Read by Kinesis Client Library applications.
* Data can be retained for a duration of up to 24 hours.
* The use cases encompass:
  * Real-time reactions to changes (such as sending a welcome email to users).
  * Analytics.
  * Insertion into derivative tables.
  * Insertion into OpenSearch Service.
  * Implementation of cross-region replication.

#### Example - Architecture Diagram

* DynamoDB Streams captures create, update, and delete operations on a table.
* (1) Kinesis Data Streams can receive the DynamoDB stream.
  * Kinesis Data Firehose can process the stream and send it to various destinations.
  * Amazon Redshift can be used for analytics queries on the DynamoDB data.
  * Amazon S3 can be used for archival of the stream's changes.
  * OpenSearch Service can index the data and enable search capabilities on the DynamoDB table.
  * Note: AWS manages most of the components in this architecture.
* (2) Custom logic can be added using a processing layer, such as a Kinesis Client Library App or a Lambda function.
  * Custom logic can include messaging, notifications using Amazon SNS, filtering, transformation, and reinserting data into DynamoDB, or sending data to OpenSearch using Lambda.

<figure><img src="../../.gitbook/assets/image (319).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### DynamoDB Streams - Selective Data, Shards, and Stream Population

* The information to be written to the stream can be selected:
  * KEYS\_ONLY - Only the key attributes of the modified item are included.
  * NEW\_IMAGE - The entire item, as it appears after modification, is included.
  * OLD\_IMAGE - The entire item, as it appeared before modification, is included.
  * NEW\_AND\_OLD\_IMAGES - Both the new and old images of the item are included.
* DynamoDB Streams are composed of shards, similar to Kinesis Data Streams.
* Shards are not provisioned by you; this process is automated by AWS.
* After enabling a stream, records are not populated retroactively.

<figure><img src="../../.gitbook/assets/image (321).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### DynamoDB Streams & AWS Lambda

* An Event Source Mapping needs to be defined to read from DynamoDB Streams.
* The appropriate permissions must be ensured for the Lambda function.
* The Lambda function is invoked synchronously.

<figure><img src="../../.gitbook/assets/image (320).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### DynamoDB TTL

• Items are automatically deleted after an expiry timestamp.

• No WCUs are consumed (i.e., no extra cost is incurred).

• The TTL attribute is required to be a “Number” data type with a “Unix Epoch timestamp” value.

• Items expired are deleted within 48 hours of expiration.

• Expired items, which haven’t been deleted, are included in reads/queries/scans (filter them out if they are not wanted).

• Expired items are removed from both LSIs and GSIs.

• A delete operation for each expired item is entered into the DynamoDB Streams (this can assist in recovering expired items).

• Use cases include reducing stored data by retaining only current items and adhering to regulatory obligations.

### DynamoDB as Session State Cache

* DynamoDB can be used for storing data as well as session state, acting as a cache for web applications. Web applications can retrieve or store session states as needed and share user logins across backend web applications.
* DynamoDB and ElastiCache serve the same purpose of storing session states.
  * ElastiCache is fully in memory, while DynamoDB is serverless and both are key/value stores.
  * If the requirement is for an in-memory session state store, ElastiCache is likely the appropriate choice.
  * If automatic scaling and other features are needed, DynamoDB is a suitable option.
* Another way to store session states is on disk, which can be achieved using EFS (Elastic File System).
  * EFS must be attached as a network drive to EC2 instances and can be an alternative to DynamoDB.
* EBS volumes and EC2 instance stores are storage options but cannot be used for shared caching across multiple instances.
* S3 can be used for session states, but it has higher latency and is more suitable for storing big files rather than small objects.
* The recommended options for session state storage are DynamoDB, ElastiCache, and EFS, with DynamoDB and ElastiCache being preferable.
* The choice depends on whether an in-memory solution or a more serverless approach with automatic scaling is desired.

### DynamoDB Operations

<figure><img src="../../.gitbook/assets/image (287).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* Copying a DynamoDB Table (Option 1):
  * :warning::warning:\[Data Pipeline] The copying of a DynamoDB Table across accounts, regions, or places can be achieved through two options.
    * AWS Data Pipeline is used as the first option.
      * An Amazon EMR Cluster is launched by Data Pipeline in the backend.
      * The DynamoDB Table is read by EMR using a scan operation and the data is written back into Amazon S3 for storage.
      * In the second step, the data is read back from Amazon S3.
      * The data is then inserted back into a **new DynamoDB Table**.

<figure><img src="../../.gitbook/assets/image (286).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### DynamoDB and S3

* Two ways of using DynamoDB with Amazon S3 are discussed.
* The first method involves storing large objects in DynamoDB.
  * DynamoDB tables have a limit of 400 kilobytes for data storage, making it unsuitable for storing images and videos.
  * Instead, an Amazon S3 bucket is used to store large objects.
  * The process involves uploading an object to Amazon S3 and storing its metadata in DynamoDB.
  * The metadata stored in DynamoDB includes the product ID, product name, and an image URL pointing to Amazon S3.
  * This approach allows for small amounts of data to be stored in DynamoDB while the large objects are stored in Amazon S3.
  * Clients retrieve the metadata from DynamoDB and fetch the corresponding objects from Amazon S3.
  * This strategy can be scaled to handle multiple products.
  *   The combination of Amazon S3 and DynamoDB leverages the strengths of each service.\
      \\

      <figure><img src="../../.gitbook/assets/image (288).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>
* Another synergy between the two services is using DynamoDB to index S3 object metadata.
  * Objects are uploaded to Amazon S3, and a Lambda function stores their metadata in a DynamoDB table.
  * Querying the DynamoDB table is easier than querying an S3 bucket for building queries and retrieving specific information about the objects.
  * Examples of queries include finding objects by a specific timestamp, calculating total storage used by a customer, listing objects based on attributes, or finding objects uploaded within a date range.
  * The necessary objects are retrieved from the S3 bucket based on the results obtained from DynamoDB.

<figure><img src="../../.gitbook/assets/image (289).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### DynamoDB - Security & Other Features

**Security**

* VPC Endpoints available to access DynamoDB without using the Internet.
* Access fully controlled by IAM.
* Encryption at rest using AWS KMS and in-transit using SSL/TLS.

**Backup and Restore feature available**

* Point in time recovery (PITR) like RDS

**Global tables**

* Multi-region, multi-active, fully replicated, high performance

**DynamoDB Local**

* Develop and test apps locally without accessing the DynamoDB web service (without Internet)

### DynamoDB - Users Interact with DynamoDB Directly

<figure><img src="../../.gitbook/assets/image (284).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* Using Web Identity Federation or Cognito Identity Pools, each user gets AWS credentials.
* You can assign an IAM Role to these users with a Condition to limit their API access to DynamoDB.
* LeadingKeys: limit row-level access for users on the Primary Key.
* Attributes: limit specific attributes the user can see.

<figure><img src="../../.gitbook/assets/image (285).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### References

Be a Better Dev. _What is a DynamoDB GSI (Global Secondary Index)_. \[Videoaula]. YouTube, 2019b. Disponível em: <[https://www.youtube.com/watch?v=ihMOlb8EZKE](https://www.youtube.com/watch?v=ihMOlb8EZKE)>. Acesso em: 8 out. 2023.
