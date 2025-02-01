# SHANNON AWS Certified Cloud Practitioner CLF-C01

### Comparison IaaS, PaaS, SaaS and On-Premises

<figure><img src="../.gitbook/assets/image (123).png" alt=""><figcaption><p>Font: PIPER; CLINTON, 2019</p></figcaption></figure>

### Cloud Deployment Types

* Cloud-Based Deployment (Public Cloud)
* On-Premise Cloud Deployment (Private Cloud)
* Hybrid Cloud Deployment
  * Examples of usage of this deployment: Bursting Up.
    *   Dynamically scale resources from a private cloud to a public cloud when additional capacity is required.

        Let's consider a retail company that operates primarily on a private cloud infrastructure hosted in their own data centers. During normal business operations, their private cloud infrastructure adequately handles the average customer demand. However, during seasonal sales or promotional events, the website experiences a significant increase in traffic, requiring additional computing resources to handle the surge in user activity.

        To address this situation, the retail company can implement a hybrid cloud architecture. They can maintain their day-to-day operations on their private cloud, ensuring data security and compliance. However, when traffic spikes occur, they can leverage bursting up capabilities to dynamically scale their resources by extending their infrastructure into the public cloud, such as utilising AWS's public cloud services.

### AWS Well-Architected Framework

The AWS Well-Architected Framework is a set of best practices and guidelines developed by Amazon Web Services (AWS) to help customers design and build secure, high-performing, resilient, and efficient cloud-based architectures.

It provides a structured approach for evaluating architectures, identifying areas of improvement, and making informed decisions when designing and operating workloads in the AWS cloud.

#### **1. Operational Excellence Design**

* Perform operation as **CODE.**
* Make frequent, small, reversible changes.
* Learn from operational failures.
* Anticipate failure.
* Refine operations procedures frequently

#### 2. Security Design

* Enable traceability.
* Keep People away from data.
* Implement strong identity foundation.
* Protect data in transit and rest.

#### 3. Reliability Design

* Test recovery procedures (verify if you can restore your backup).
* Stop guessing capacity.
* Scale horizontally to increase aggregate workload availability.
* Manage change in automation.

#### 4. Performance Design

* Go global in minutes.
* Experiment more often.
* access advanced tech.

#### 5. Cost Optimisation

* Implement cloud financial management.
* Analyse and attribute expenditure.

### AWS Shared Responsibility Mode

* Customers have complete control over their content and are responsible for managing critical content security:
  * Content stored on AWS.
  * Country where content is stored.
  * Access to that content.
  * “from layer three upwards”.
* AWS Responsibilities:
  * 24/7 security staffing.
  * Undisclosed locations.
  * “from Layer two downwards”.

### AWS Cloud Value Propositions

* Cost:
  * Eliminates large upfront investments for: cabling, cooling, power, network, racks, servers, certifications and labor.
  * Pay-as-you-go and pay-as-you-use.
* Agility
  * Users can leverage the infrastructure for speed, experimentation and innovation.
* “Universal” Access
  * Represents anywhere, anytime access to resources.
* Elasticity
  * Unlimited scalability and dynamic provisioning & processioning of processing, storage, database, and memory.
  * Allows organisations to shift and pool resources over different infrastructures to avoid over-provisioning and cost overruns.
  * Scalability is ≠ than elasticity as it increases (**only**) workloads over existing HW resources only (elasticity increases VMs on existing HW).
* Demand-Driven service
  * It allows users to provision cloud resources as needed automatically and rapidly without human interaction in most scenarios.

### Elasticity through Auto-scaling

* Auto Scaling monitors your applications and automatically adjusts capacity to maintain steady, predictable performance at the lowest possible cost

### Key Aspects Cloud Economics

#### Pay-as-you-go

* charging model that allows you to pay based on compute and storage resources used or by the second/minute/hour.

#### Metering

* The ability of provider to automate the tracking of all resources to support the measured usage service.
* facilitates the pay-as-you-go and demand-driven features of CSPs for billing, monitoring, and reporting.

#### Cloud Bursting

* The process of running applications on internal resources or a private cloud then “bursting up” to a public/hybrid cloud solution

#### Chargeback

* accounting approach that decentralizes the cost of it services and applies them to the budgets of the teams or business units.
* eliminates the need to consolidate all service costs under one bill.
* Many private clouds and internal IT departments will use the term “showback” instead.

### AWS CAF (Cloud adoption Framework)

* CAF organizes guidance at a high level into six areas of focus, called **perspectives**.
* Each perspective focuses on discrete responsibilities
* Business capabilities: Business, people, governance perspectives
* Technical capabilities: Operations, Security, Platform perspectives

### AWS Compliance

#### AWS Artifact

* AWS Artifact is a compliance information resource.
* \*\*\*\*AWS Artifact provides access to official documentation on the compliance of AWS infrastructure relating to any one of dozens of government or industry security standards.
* AWS Artifact’s Components:
  * Artifact Agreements:
    * assists with signing agreements with AWS regarding your use of certain types of information throughout AWS service.
  * Artifact Reports:
    * helps with providing compliance reports from third-party auditors.

> Will the application you plan to run on AWS be processing credit card transactions? What about private health records, personal employment histories, or restricted military information? Are you sure the AWS security and reliability environment is good enough to meet the regulatory standards required by your industry and government?

But where can you find authoritative answers? **AWS Artifact**

>

### AWS Customer Compliance Center

* What does it have?
  * FAQ.
  * Overviews of how to do risk management on the cloud.

### AWS Organisations

* It provides policy-based management for multiple AWS accounts.
  * Create groups of accounts.
  * Automated account creation using console and APIs.
  * Apply and manage policies for account groups.
* Single consolidated bill
* SCPs (Service Control Policies)

<figure><img src="../.gitbook/assets/image (124).png" alt=""><figcaption><p>Font: SHANNON, 2023</p></figcaption></figure>

### AWS Control Tower

Use AWS Control Tower to easy set up and manage a secure, multi-account environment based on established best practices.

Excellent solution if you are:

* Building a new AWS environment
* Beginning a journey to AWS
* Launching a new cloud initiative
* Working with existing accounts

### Service Pricing

* pay-as-you-go pricing for over 120 cloud services.
  * This reduces the risk of over-positioning or missing capacity.
* save when you commit.
  * save up to 75% over equivalent on-demand capacity.
* pay less by using more.
* free-tier
  * some of the trust advisor features
  * shield

#### AWS Pricing Calculator

* AWS tool to estimate the cost of AWS products and services.
* View estimated costs per service, service group, and totals.
* You use this service before migrating to the cloud.

#### AWS Cost Explore

* Provide information of the cost involved in the services you use on AWS.

### AWS Support Models

#### Business and Enterprise Plans

* AWS Shield Advanced “free”
* 24/7 support teams
* All features of AWS Trusted Advisor
  * 115 advisor checks.
  * cost optimisation (14).
  * performance (10).
  * security (17).
  * fault tolerance (24).
  * service limits (50).
*   **Enterprise Plans**

    * Concierge Support Team\\

    <figure><img src="../.gitbook/assets/image (125).png" alt=""><figcaption><p>Font:</p></figcaption></figure>

### Key Networking Terms

#### EOIG

“NAT Gateway” for IPV6.

### Core Networking Services

#### Elastic Load Balancing (ELB)

* Application LB
* Network LB
*   Gateway LB\
    \\

    <figure><img src="../.gitbook/assets/image (127).png" alt=""><figcaption><p>Font: SHANNON, 2023</p></figcaption></figure>

> Usually, the target of an ELB is a public subnet that is auto scaling instances.

#### AWS Route 53

AWS’s DNS web service. Route 53 effectively connects user requests to infrastructure running in AWS (e.g., EC2 instances, Elastic Load Balancing load balancers, or Amazon S3 buckets) and can also be used to route users to infrastructure outside of AWS.

#### CloudFront

Is a Content Delivery Network (CDN).

<figure><img src="../.gitbook/assets/image (128).png" alt=""><figcaption><p>Font: SHANNON, 2023</p></figcaption></figure>

#### AWS Direct Connect

{% content-ref url="../aws-direct-connect-vs-aws-vpn.md" %}
[aws-direct-connect-vs-aws-vpn.md](../aws-direct-connect-vs-aws-vpn.md)
{% endcontent-ref %}

#### AWS Site-to-Site VPN

{% content-ref url="../aws-direct-connect-vs-aws-vpn.md" %}
[aws-direct-connect-vs-aws-vpn.md](../aws-direct-connect-vs-aws-vpn.md)
{% endcontent-ref %}

### Core Compute Services

#### Amazon Elastic Compute Cloud (Amazon EC2)

* EC2 is a web service that provides secure, resalable compute capacity in the cloud.

**Instances Types**

* On-demand: you pay for compute capacity by the hour with no long-term commitments.
* Spot: allow you to bid on spare Amazon EC2 computing capacity.
* Reserved: (up to 75% cheaper compared to **on-demand**).
* Convertible Reserved Instances: They allow you to change the instance family and other parameters associated with a reserved instance at any time (45% cheaper compared to **on-demand**).

#### SNS and SQS

* Amazon Simple No\&fica\&on Service (SNS) is a publish/subscribe service.
  * Using Amazon SNS topics, a publisher distributes messages to subscribers.
* Amazon Simple Queue Service (SQS) is a message queuing service that lets you send, store, and receive messages between socware components, without losing messages or requiring other services to be available.

#### Elastic Container Registry (ECR)

* ECR is a fully managed Docker container registry that makes it easy for developers to store, manage, and deploy Docker container images.

**Elastic Container Service (ECS)**

* It is a highly scalable container orchestration service that supports Docker containers. It allows the execution ans cake if containerised applications.

**Elastic Container Service for Kubernetes**

#### AWS Elastic Beanstalk

This service provide the a way to deploy web applications and services developed in several different platforms.

* Platform: Generic Docker, Preconfigured Docker.
* Upload an application.

#### AWS Batch

This service provide a way yo run hundreds of thousands of batch computing jobs on AWS.

#### EC2 Auto Scaling

* EC2 Auto Scaling helps you preserve application availability and lets you to automatically add or remove EC2 instances according to conditions that you describe.
* You can also use the **dynamic** and **predictive** (a schedule is set to perform the scaling operation) scaling features of EC2 Auto Scaling to add or remove EC2 instances.

### Core Storage Services

#### Block Storage

* Files are split up and stored in fixed-sized blocks.
* Host databases, supporting random read/write operations, and keeping system files of the running virtual machines.
* Capacity increased by adding more nodes.
* Suitable for apps that need high IOPS, database, and transactional data.

**Main AWS Service:** **Elastic Block Storage (EBS)**

* Each Amazon EBS volume is automatically replicated within its Availability Zone to protect you from component failure, offering high availability and durability.

#### Object Storage Concepts - S3

* Data is stored as discrete objects.
* Data is not placed in a hierarchy of directories and instead resides in a flat address space.
* Applica\&ons iden\&fy the discrete data objects by a unique address.
* Designed for access at the applica\&on level using an API rather than at the user level.
* Each object may also have metadata that is retrieved with it to better define or classify rela\&onships with other objects.

**Main AWS Service: S3**

* S3 is designed for 99.999999999% (11 9's) of durability.
* S3 Glacier is a low-cost, highly-durable (99.999999999%), and secure long-term backup and archiving solu\&on.

**Main AWS Service: EFS**

Amazon Elastic File System (EFS) is a fully managed, cloud-based file storage service that provides NFS shared file system storage to Linux workloads (AWS, 2023a).

**Main AWS Service: Storage Gateway**

AWS Storage Gateway is a hybrid cloud storage service that provides on-premises access to virtually unlimited cloud storage (AWS, 2023b).

Storage Gateway supports four key hybrid cloud use cases: move backups and archives to the cloud, reduce on-premises storage with cloud-backed file shares, provide on-premises applications low-latency access to data stored in AWS, and data lake access for pre and post-processing workflows (AWS, 2023b).

### Core Database Services

#### Database Comparisons

<figure><img src="../.gitbook/assets/image (130).png" alt=""><figcaption><p>Font: <a href="https://docs.aws.amazon.com/whitepapers/latest/aws-overview/database.html">AWS's Page</a></p></figcaption></figure>

#### AuroraDB

* Fully-managed MySQL and Postgress compa\&ble rela\&onal database engine.

#### Amazon ElastiCache

* The service improves the performance of web applications by empowering one to retrieve information from fast, managed, in-memory caches, instead of relying entirely on slower disk-based databases.

#### Amazon Redshift

* data warehosuse.

#### AWS Database Migration Service (AWS DMS)

* Usecase: “OracleDB” and need to migrate to cloud.
  * RDS OracleDB + AWS DMS.

### Amazon Cloudwatch

* Monitor applications.
* Recognize and respond to system-wide performance changes.
* Optimize resource utilization.
* Gain a unified view of operational health.

### AWS CloudTrail

CloudTrail, customers can log, con\&nuously monitor, and retain account ac\&vity related to all API calls across the AWS infrastructure.

### References

* Piper, Ben; Clinton, David. _AWS Certified Cloud Practitioner Study Guide: CLF-C01 Exam_. 1ª edição. Sybex, 2019.
* Shannon, Michael J. AWS Cloud Practitioner CLF-C02 Crash Course. O'Reilly. \[Videoaula]. Disponível em: https://learning.oreilly.com/live-events/aws-cloud-practitioner-clf-c01-crash-course. Acesso em: 8 set. 2023.
* AWS. _O que é o Amazon Elastic File System (Amazon EFS)?_ Amazon Web Services. Disponível em: [https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html). Acesso em: 8 set. 2023a.
* AWS. _AWS Storage Gateway_. Amazon Web Services. Disponível em: [https://aws.amazon.com/storagegateway/](https://aws.amazon.com/storagegateway/). Acesso em: 8 set. 2023b.
