# Exam Questions CLF-C01

mpany is planning to run a global marketing application in the AWS Cloud. The application will feature videos that can be viewed by users. The company must ensure that all users can view these videos with low latency.\
Which AWS service should the company use to meet this requirement?

* A. AWS Auto Scaling
* B. Amazon Kinesis Video Streams
* C. Elastic Load Balancing
* D. Amazon CloudFront

<details>

<summary>Answer</summary>

D

</details>

Which pillar of the AWS Well-Architected Framework refers to the ability of a system to recover from infrastructure or service disruptions and dynamically acquire computing resources to meet demand?\\

* A. Security
* B. Reliability
* C. Performance efficiency
* D. Cost optimization

<details>

<summary>Answer</summary>

B

</details>

3. Which of the following are benefits of migrating to the AWS Cloud? (Choose two.)

* A. Operational resilience
* B. Discounts for products on Amazon.com
* C. Business agility
* D. Business excellence
* E. Increased staff retention

<details>

<summary>Answer</summary>

A and C

</details>

A company is planning to replace its physical on-premises compute servers with AWS serverless compute services. The company wants to be able to take advantage of advanced technologies quickly after the migration.\
Which pillar of the AWS Well-Architected Framework does this plan represent?

* A. Security
* B. Performance efficiency
* C. Operational excellence
* D. Reliability

<details>

<summary>Answer</summary>

B

</details>

A large company has multiple departments. Each department has its own AWS account. Each department has purchased Amazon EC2 Reserved Instances. Some departments do not use all the Reserved Instances that they purchased, and other departments need more Reserved Instances than they purchased. The company needs to manage the AWS accounts for all the departments so that the departments can share the Reserved Instances. Which AWS service or tool should the company use to meet these requirements?

* A. AWS Systems Manager
* B. Cost Explorer
* C. AWS Trusted Advisor
* D. AWS Organizations

<details>

<summary>Answer</summary>

D

</details>

Which component of the AWS global infrastructure is made up of one or more discrete data centers that have redundant power, networking, and connectivity?

* A. AWS Region
* B. Availability Zone
* C. Edge location
* D. AWS Outposts

<details>

<summary>Answer</summary>

B

An Availability Zone is a single data center or a group of data centers within a Region.

</details>

Which duties are the responsibility of a company that is using AWS Lambda? (Choose two.)

* A. Security inside of code
* B. Selection of CPU resources
* C. Patching of operating system
* D. Writing and updating of code
* E. Security of underlying infrastructure

<details>

<summary>Answer</summary>

A and D

</details>

Which AWS services or features provide disaster recovery solutions for Amazon EC2 instances? (Choose two.)

* A. ׀•׀¡2 Reserved Instances
* B. EC2 Amazon Machine Images (AMIs)
* C. Amazon Elastic Block Store (Amazon EBS) snapshots
* D. AWS Shield
* E. Amazon GuardDuty

<details>

<summary>Answer</summary>

B and C

The **AMI** is created from snapshots of your instance's root volume and any other EBS volumes attached to your instance. You can use this AMI to launch a restored version of the EC2 instance.

Amazon **EBS** allows the creation of snapshots, which are point-in-time copies of EBS volumes. By taking regular snapshots of EBS volumes attached to EC2 instances, companies can create backups that can be used to restore data or launch new instances in the event of a disaster. Snapshots are stored durably and can be easily restored when needed.

</details>

A company is migrating to the AWS Cloud instead of running its infrastructure on premises.\
Which of the following are advantages of this migration? (Choose two.)

* A. Elimination of the need to perform security auditing
* B. Increased global reach and agility
* C. Ability to deploy globally in minutes
* D. Elimination of the cost of IT staff members
* E. Redundancy by default for all compute services

<details>

<summary>Answer</summary>

B and C

</details>

A user is comparing purchase options for an application that runs on Amazon EC2 and Amazon RDS. The application **cannot sustain any interruption**. The application experiences a predictable amount of usage, including some seasonal spikes that last only a few weeks at a time. It is not possible to modify the application.\
Which purchase option meets these requirements MOST cost-effectively?

* A. Review the AWS Marketplace and buy Partial Upfront Reserved Instances to cover the predicted and seasonal load.
* B. Buy Reserved Instances for the predicted amount of usage throughout the year. Allow any seasonal usage to run on Spot Instances.
* C. Buy Reserved Instances for the predicted amount of usage throughout the year. Allow any seasonal usage to run at an On-Demand rate.
* D. Buy Reserved Instances to cover all potential usage that results from the seasonal usage.

<details>

<summary>Answer</summary>

C

</details>

A company plans to create a data lake that uses Amazon S3.\
Which factor will have the MOST effect on cost?

* A. The selection of S3 storage tiers Most Voted
* B. Charges to transfer existing data into Amazon S3
* C. The addition of S3 bucket policies
* D. S3 ingest fees for each request

<details>

<summary>Answer</summary>

A

</details>

Which AWS service or feature can a company use to determine which business unit is using specific AWS resources?

* A. Cost allocation tags
* B. Key pairs
* C. Amazon Inspector
* D. AWS Trusted Advisor

<details>

<summary>Answer</summary>

A

[Cost allocation tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) are a mechanism provided by AWS to assign metadata to AWS resources for cost tracking and categorization purposes.

</details>

Which of the following are components of an AWS Site-to-Site VPN connection? (Choose two.)

* A. AWS Storage Gateway
* B. Virtual private gateway
* C. NAT gateway
* D. Customer gateway
* E. Internet gateway

<details>

<summary>Answer</summary>

B and D

</details>

A company needs to establish a connection between two VPCs. The VPCs are located in two different AWS Regions. The company wants to use the existing infrastructure of the VPCs for this connection. Which AWS service or feature can be used to establish this connection?

* A. AWS Client VPN
* B. VPC peering
* C. AWS Direct Connect
* D. VPC endpoints

<details>

<summary>Answer</summary>

B

</details>

According to the AWS shared responsibility model, what responsibility does a customer have when using Amazon RDS to host a database?

* A. Manage connections to the database
* B. Install Microsoft SQL Server
* C. Design encryption-at-rest strategies
* D. Apply minor database patches

<details>

<summary>Answer</summary>

A

Not C (Design encryption-at-rest strategies): Amazon RDS offers encryption-at-rest for database storage by default. It handles the encryption of data on disk and manages the associated keys. As a customer, you can enable encryption when creating an Amazon RDS instance, but you don't need to design the encryption strategy yourself.

</details>

A user needs to determine whether an Amazon EC2 instance's security groups were modified in the last month.\
How can the user see if a change was made?

* A. Use Amazon EC2 to see if the security group was changed.
* B. Use AWS Identity and Access Management (IAM) to see which user or role changed the security group.
* C. Use AWS CloudTrail to see if the security group was changed.
* D. Use Amazon CloudWatch to see if the security group was changed.

<details>

<summary>Answer</summary>

C

AWS CloudTrail provides a history of events for an AWS account, including API calls made to EC2 and changes made to security groups.

</details>

Which of the following are included in AWS Enterprise Support? (Choose two.)\\

* A. AWS technical account manager (TAM)
* B. AWS partner-led support
* C. AWS Professional Services
* D. Support of third-party software integration to AWS
* E. 5-minute response time for critical issues

<details>

<summary>Answer</summary>

A and D

</details>

A global media company uses AWS Organizations to manage multiple AWS accounts.\
Which AWS service or feature can the company use to limit the access to AWS services for member accounts?

* A. AWS Identity and Access Management (IAM)
* B. Service control policies (SCPs)
* C. Organizational units (OUs)
* D. Access control lists (ACLs)

<details>

<summary>Answer</summary>

B

AWS Organizations helps to manage multiple AWS accounts in a centralized manner. SCPs are a feature of AWS Organizations that allow an organization to set rules that govern the use of AWS services across all accounts in the organization. SCPs can be used to restrict the use of specific AWS services or to impose additional conditions or requirements on the use of those services. SCPs are applied at the organizational unit (OU) level, so organizations can create different policies for different groups of accounts within their AWS Organization.

</details>

A company wants to limit its employees' AWS access to a portfolio of predefined AWS resources.\
Which AWS solution should the company use to meet this requirement?

* A. AWS Config
* B. AWS software development kits (SDKs)
* C. AWS Service Catalog
* D. AWS AppSync

<details>

<summary>Answer</summary>

C

AWS Service Catalog is a service that allows organizations to create and manage catalogs of IT services, including AWS resources, that are approved for use within the organization. It enables administrators to curate a set of predefined resources and configurations that meet the organization's security, compliance, and operational standards.

</details>

A company is developing a mobile app that needs a high-performance NoSQL database.\
Which AWS services could the company use for this database? (Choose two.)

* A. Amazon Aurora
* B. Amazon RDS
* C. Amazon Redshift
* D. Amazon DocumentDB (with MongoDB compatibility)
* E. Amazon DynamoDB

<details>

<summary>Answer</summary>

D and E

Amazon DocumentDB is a fully managed, highly scalable, and highly available document database service that is compatible with MongoDB.

</details>

According to AWS, what is the benefit of Elasticity?

* **A.** Minimize storage requirements by reducing logging and auditing activities].
* **B.** Create systems that scale to the required capacity based on changes in demand.
* **C.** Enable AWS to automatically select the most cost-effective services.
* **D.** Accelerate the design process because recovery from failure is automated, reducing the need for testing.

<details>

<summary>Answer</summary>

B

</details>

What is the AWS feature that enables fast, easy and secure transfers of files over long distances between your client and your Amazon S3 bucket?

* **A.** File Transfer
* **B.** HTTP Transfer
* **C.** Amazon S3 Transfer Acceleration
* **D.** S3 Acceleration

<details>

<summary>Answer</summary>

C

Amazon S3 Transfer Acceleration enables fast, easy, and secure transfers of files over long distances between your client and an S3 bucket. Transfer Acceleration takes advantage of Amazon CloudFront’s globally distributed edge locations. As the data arrives at an edge location, data is routed to Amazon S3 over an optimized network path.

</details>

Which of the following services allows you to analyze EC2 Instances against pre-defined security templates to check for vulnerabilities?

* **A.** AWS Trusted Advisor
* **B.** AWS Inspector
* **C.** AWS WAF
* **D.** AWS Shield

<details>

<summary>Answer</summary>

B

Amazon Inspector enables you to analyze the behavior of your AWS resources and helps you to identify potential security issues. Using Amazon Inspector, you can define a collection of AWS resources that you want to include in an assessment target. You can then create an _assessment template_ and launch a security _assessment run_ of this target.

</details>

A website for an international sport governing body would like to serve its content to viewers from different parts of the world in their vernacular language. Which of the following services provide location-based web personalization using geolocation headers?

* **A.** Amazon CloudFront
* **B.** Amazon EC2 Instance
* **C.** Amazon Lightsail
* **D.** Amazon Route 53

<details>

<summary>Answer</summary>

A

Amazon CloudFront supports country-level location-based web content personalization with a feature called Geolocation Headers.

You can configure CloudFront to add additional geolocation headers that provide more granularity in your caching and origin request policies. The new headers give you more granular control of cache behavior and your origin access to the viewer’s country name, region, city, postal code, latitude, and longitude, all based on the viewer’s IP address.

![](<../.gitbook/assets/image (152).png>)\\

</details>

A company wants to utilize AWS storage. For them, low storage cost is paramount. The data is rarely retrieved and a data retrieval time of 13-14 hours is acceptable for them. What is the best storage option to use?

* A. Amazon S3 Glacier
* B. S3 Glacier Deep Archive
* C. Amazon EBS volumes
* D. AWS CloudFront

<details>

<summary>Answer</summary>

B

<img src="../.gitbook/assets/image (153).png" alt="" data-size="original">

</details>

### References

* [Examtopics - Cloud Practitioner](https://www.examtopics.com/exams/amazon/aws-certified-cloud-practitioner/view/5/)
* [Whizlabs - Cloud Practitioner](https://www.whizlabs.com/blog/aws-cloud-practitioner-certification-questions/)
