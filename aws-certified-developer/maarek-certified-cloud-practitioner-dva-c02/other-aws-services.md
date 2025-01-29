# Other AWS Services

### SES

**TODO**

### Amazon OpenSearch Service

It is the successor to Amazon ElasticSearch.

AWS OpenSearch, formerly known as Amazon ElasticSearch, underwent a name change due to licensing issues. It’s a powerful tool that complements other databases by providing advanced search capabilities.

**Comparison with DynamoDB:** DynamoDB allows data querying only by primary key or via indexes. In contrast, OpenSearch enables searching any fields, even for partial matches, making it a popular choice for providing search functionality to applications.

**Capabilities of OpenSearch:** Despite its name, OpenSearch isn’t limited to search operations. It also supports analytic queries, offering two modes for provisioning an OpenSearch Cluster:

1. **Managed Cluster Option:** Actual physical instances are provisioned and visible to you.
2. **Serverless Cluster:** All aspects from scaling to operations are handled by AWS.

OpenSearch uses its own query language and doesn’t natively support SQL. However, SQL compatibility can be enabled via a plugin.

**Data Ingestion:** OpenSearch can ingest data from various sources like Kinesis Data Firehose, IoT, CloudWatch Logs, or custom-built applications.

**Security:** Security is ensured through integration with Cognito and IAM, along with at-rest and in-flight encryption.

**Analytics:** OpenSearch Service supports analytics. You can use OpenSearch Dashboards to create visualizations on top of your OpenSearch Data.

**Common Usage Patterns:**

1. **DynamoDB with OpenSearch:** DynamoDB holds your data where users insert, delete, and update data. The streams in DynamoDB Stream are picked up by a Lambda Function which inserts the data into Amazon OpenSearch in real time. This allows your application to search for specific items and retrieve full items from your DynamoDB Table.
2. **CloudWatch Logs with OpenSearch:** You can use a CloudWatch Log Subscription Filter to send data in real time to a Lambda Function managed by AWS which sends all the data into Amazon OpenSearch.
3. **Kinesis with OpenSearch:** Kinesis Data Streams can be sent into Amazon OpenSearch using either Kinesis Data Firehose for near real-time service or a Lambda Function for real-time service.

### Amazon Athena

Amazon Athena is a serverless query service that allows you to analyze data stored in Amazon S3 buckets using standard SQL language. It’s built on the Presto engine, which also uses SQL.

**Data Analysis:** Users load data into an S3 bucket, and then use the Athena service to query and analyze this data directly in Amazon S3 without moving it.

**Serverless:** Athena is serverless, meaning it directly analyzes your data in your S3 bucket without the need for provisioning any databases.

**Data Formats:** Athena supports various data formats such as CSV, JSON, ORC, Avro, Parquet, and possibly others.

**Pricing:** The pricing model is simple - you pay a fixed amount per terabytes of data scanned.

**Integration with QuickSight:** Athena is often used in conjunction with Amazon QuickSight to create reports and dashboards. QuickSight connects to Athena, which in turn connects to your S3 buckets.

**Use Cases:** Amazon Athena is used for ad hoc queries, business intelligence, analytics, reporting, and analyzing logs originating from your AWS services such as VPC flow logs, load balancer logs, CloudTrail trails, etc.

:warning::warning::warning::warning::warning: **Exam Tip:** If you need to analyze data in Amazon S3 using a serverless SQL engine, consider using Athena.

#### Performance Improvement

The key points about performance improvements in Amazon Athena:

* **Data Scanning:** Use a data type that requires scanning less data to reduce costs. **Columnar data types are recommended** as they only scan the columns you need.
* **Recommended Formats:** Apache Parquet and ORC are the recommended formats for Amazon Athena due to their significant performance improvements.
* **Data Conversion:** Services like Glue can be used to convert your files into the Apache Parquet or ORC format.
* **Data Compression:** Compress data for smaller retrievals to scan less data. Various compression mechanisms can be used.
* **Data Partitioning:** Partition your datasets if you frequently query specific columns. This allows Athena to know exactly which folder and path in Amazon S3 needs to be scanned for data.
* **File Size:** Use larger files to minimize overheads. Larger files (e.g., 128 megabytes and over) are easier to scan and retrieve, improving Athena’s performance.

#### Federated Query

The key points about the Federated Query feature in Amazon Athena:

* **Federated Query:** Allows querying data not only in S3 but also in other locations like relational or non-relational databases, objects, and custom data sources on AWS or on-premises.
* **Data Source Connector:** A Lambda function that runs the Federated Queries in other services such as CloudWatch Logs, DynamoDB, RDS, etc.
* **Query Execution:** Queries can be executed across various services like ElastiCache, Document DB, DynamoDB, Redshift, Aurora, SQL Server, MySQL, HBase on the EMR service, or any on-premises database.
* **Result Storage:** The results of these queries can be stored in Amazon S3 buckets for later analysis. This allows for extensive data analysis and querying across multiple platforms.

### Amazon MSK

* **What is Amazon MSK?** Amazon MSK provides a fully-managed Kafka cluster on AWS. It allows you to create, update, and delete clusters on the fly.
* **Management of Kafka and Zookeeper Nodes:** Amazon MSK creates and manages Kafka broker nodes and Zookeeper broker nodes in your cluster.
* **Deployment:** You can deploy the cluster in your VPC, across multiple Availability Zones (up to three) for high availability.
* **Automatic Recovery:** Amazon MSK provides automatic recovery from common Kafka failures.
* **Data Storage:** The data is stored on EBS volumes for as long as you want.
* **Ease of Setup:** Setting up Apache Kafka can be challenging. With Amazon MSK, you can deploy Kafka on AWS with just one click.
* **MSK Serverless:** In addition to the standard service, you also have the option to use MSK Serverless. With this option, you don’t need to provision servers or manage capacity. Amazon MSK will automatically provision resources and scale compute and storage for you.

#### The differences between Kinesis Data Streams and Amazon MSK:

* **Message Limit:** Kinesis Data Streams have a 1 megabyte message limit. This is also the default in Amazon MSK, but it can be configured for higher message retention, for example, 10 megabytes.
* **Data Streams and Shards:** In Kinesis Data Streams, you can have Data Streams with Shards. In Amazon MSK, the equivalent concept is Kafka Topics with Partitions.
* **Scaling:** To scale a Kinesis Data Stream, you need to perform Shard Splitting and Merging. In Amazon MSK, to scale a topic, you can only add partitions; removing partitions is not possible.
* **In-flight Encryption:** Kinesis Data Streams support in-flight encryption. For Amazon MSK, you have either plaintext or TLS in-flight encryption.
* **At-rest Encryption:** Both Kinesis Data Streams and Amazon MSK support at-rest encryption.

#### Amazon MSK Consumers

<figure><img src="../../.gitbook/assets/image (269).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* **Kinesis Data Analytics for Apache Flink:** You can use a Flink Application to read directly from the MSK cluster.
* **Glue for Streaming ETL Jobs:** Glue can be used to perform streaming ETL jobs, which are powered by Apache Spark Streaming.
* **Lambda Functions:** Lambda functions can be used to have Amazon MSK as a direct event source.
* **Custom Kafka Consumer:** You can write your own Kafka consumer and run it on any platform you prefer, such as Amazon EC2 instances, an ECS cluster, or an EKS cluster.

### Amazon Macie

* Amazon Macie is a fully managed data security and data privacy service that uses machine learning and pattern matching to discover and protect your sensitive data in AWS.
* Macie helps identify and alert you to sensitive data, such as personally identifiable information (PII)

<figure><img src="../../.gitbook/assets/image (270).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### AWS AppConfig

* **Dynamic Configurations:** AWS AppConfig allows you to configure, validate, and deploy dynamic configurations.
* **Independent Deployments:** You can deploy dynamic configuration changes to your applications independently of any code deployments.
* **No Restart Needed:** There’s no need to restart the application when deploying configuration changes.
* **Features:** AWS AppConfig supports feature flags, application tuning, and allow/block listing.
* **Compatibility:** AWS AppConfig can be used with applications on EC2 instances, Lambda, ECS, EKS, and more.
* **Gradual Deployment:** You can gradually deploy the configuration changes and rollback if any issues occur.
* **Validation Before Deployment:** AWS AppConfig allows you to validate configuration changes before deployment using either:
  * JSON Schema for syntactic checks, or
  * Lambda Function to run code for semantic checks. This ensures that your configurations are both syntactically and semantically correct before they are deployed. This should provide a comprehensive understanding of AWS AppConfig.

<figure><img src="../../.gitbook/assets/image (271).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudWatch Evidently

<figure><img src="../../.gitbook/assets/image (272).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* **Feature Validation:** CloudWatch Evidently allows you to safely validate new features by serving them to a specified percentage of your users.
* **Risk Reduction:** It helps reduce risk and identify unintended consequences of new features.
* **Data Collection and Analysis:** CloudWatch Evidently collects experiment data, analyzes it using statistical methods, and monitors performance.
* **Launches (Feature Flags):** This feature enables and disables features for a subset of users. It allows you to test the impact of a feature on a small group before rolling it out to all users.
* **Experiments (A/B Testing):** This feature allows you to compare multiple versions of the same feature. It helps determine which version is more effective or provides a better user experience.
* **Overrides:** This feature allows you to pre-define a variation for a specific user. It gives you control over the user’s experience for testing purposes.
* **Event Storage:** Evaluation events are stored in CloudWatch Logs or S3 for future reference and analysis.
