# Kinesis

### Use Cases and Their Partition Key Choices

[In Kinesis, the partition key is used to isolate and route records to different shards within a data stream](https://aws.amazon.com/kinesis/data-streams/faqs/). Below are some of the use cases and how they might choose partition keys:

1. **Customer Data Tracking**: If an application is tracking data per customer, it might use the `customer_id` as the partition key. [This would ensure all data for a particular customer ends up in the same shard, maintaining the order of records for each customer](https://docs.aws.amazon.com/firehose/latest/dev/dynamic-partitioning.html).
2. [**Transaction Tracking**: In a financial application, you might use `transaction_id` as the partition key to ensure all data related to a particular transaction is kept in order](https://docs.aws.amazon.com/firehose/latest/dev/dynamic-partitioning.html).
3. **Log Data**: If you’re streaming log data, you might use the `log_level` or `server_id` as the partition key to group similar logs together.
4. **IoT Device Data**: For an IoT application, you could use `device_id` as the partition key to ensure all data from a particular device ends up in the same shard.

Kinesis computes the MD5 hash of a partition key to decide what shard to store the record on. [If all of your records use the same partition key then you’re still writing to a single shard, because they’ll all have the same hash value](https://stackoverflow.com/questions/48399903/what-is-partition-key-in-aws-kinesis-all-about). Therefore, it’s important to choose a partition key that aligns with data distribution and consumption patterns.

<details>

<summary>ChatGPT Output for "Use Cases and Their Partition Key Choices for Kines"</summary>

1. **IoT Data Ingestion**:
   * Use Case: A company wants to ingest data from thousands of IoT devices into a Kinesis stream.
   * Partition Key: The unique device identifier (e.g., MAC address or serial number) could be used as the partition key. This ensures that data from each device is processed independently and distributed evenly across shards.
2. **User Activity Tracking**:
   * Use Case: An online e-commerce platform needs to track user activity, such as page views, clicks, and purchases.
   * Partition Key: The user ID or session ID can serve as the partition key. This ensures that all events related to a specific user are sent to the same shard for personalized analytics.
3. **Log Aggregation**:
   * Use Case: An organization wants to aggregate logs from multiple servers into a centralized system for monitoring and analysis.
   * Partition Key: A combination of the server hostname and timestamp can be used as the partition key. This allows logs from each server to be grouped together while maintaining chronological order.
4. **Financial Data Streaming**:
   * Use Case: A financial institution needs to process high-frequency trading data in real-time.
   * Partition Key: A combination of the financial instrument symbol and timestamp can be used as the partition key. This ensures that data for each instrument is processed independently and ordered by time.
5. **Social Media Stream**:
   * Use Case: A social media platform wants to analyze real-time user posts, likes, and comments.
   * Partition Key: The user's account ID or post ID can be used as the partition key. This ensures that interactions related to a specific user or post are processed together.
6. **Sensor Data in a Smart Grid**:
   * Use Case: A utility company collects data from sensors in a smart grid to monitor power distribution.
   * Partition Key: The location ID of each sensor (e.g., grid coordinates) can be used as the partition key. This allows the company to analyze data by location.
7. **Video Stream Ingestion**:
   * Use Case: A video streaming service wants to ingest and process live video streams from cameras.
   * Partition Key: The camera ID or stream ID can serve as the partition key. This ensures that each camera's feed is processed independently.
8. **Elasticsearch Log Ingestion**:
   * Use Case: Log data needs to be ingested into Elasticsearch for indexing and searching.
   * Partition Key: A combination of log type and timestamp can be used as the partition key. This allows logs of the same type to be grouped together.
9. **Anomaly Detection**:
   * Use Case: An organization wants to perform real-time anomaly detection on various data sources.
   * Partition Key: Depending on the data sources, a combination of attributes that define the data source and time window can be used as the partition key.

</details>
