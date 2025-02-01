# 4. Core AWS Services
## Administrative, Monitoring, and Security Services
### CloudWatch
CloudWatch is the AWS service for monitoring and measuring services running within the AWS environment.
It provides data about:
- application performance 
- how it may change over time
- resource utilization
- centralized and consolidated view of the overall health of systems and services. 

CloudWatch even has agents that can be used to monitor on-premises systems, along with those hosted in AWS, for those that are using a hybrid cloud model. 

**CloudWatch will keep data for up to 15 months.**

CloudWatch also provide alarms capabilities. Alarms are based on predefined thresholds or through the use of machine learning algorithms and can trigger automated processes as a result. 

    - Alarms can be configured to perform auto-scaling tasks based upon loads or other threshold.
    - Alarms can trigger workflows across the different AWS services

### CloudTrail
CloudTrail is the AWS service for performing **auditing and compliance within your AWS account**. CloudTrail pairs with CloudWatch to analyze all the logs and data collected from the services within your account, which can then be audited and **monitored for all activities done by users and admins within your account**. 

**CloudTrail will log all account activities performed**, regardless of the method through which they were done. It logs all activity through the Management Console, CLI, and any API calls that are made, along with the originating IP address and all time and date data.

CloudTrail also has the ability to trigger automation processes based on detected events. For example, if a certain type of API call is made, CloudTrail can trigger certain alerts to happen, or even the initiation of policies or other service changes.

### Shield
AWS Shield provides protection from and mitigation of DDoS attacks on AWS services.

AWS Shield comes in two different service categories: Standard and Advanced.

#### Standard
Standard coverage is provided at **no additional charge** and is designed to **protect against common DDoS attacks**, especially for any accounts utilizing CloudFront and Route 53. This will protect websites and applications from the most frequently occurring attacks and virtually all known attacks on **Layer 3 and 4 against CloudFront and Route 53**.

#### Advanced
For accounts that use more complex and large systems across EC2, ELB, and many other services, there is an Advanced tier of service that comes at an additional cost.

More sophisticated mitigation capabilities, along with 24-hour access to the AWS DDoS Response team.

### WAF
AWS WAF is a **web application firewall** that protects web applications against many common attacks. 

Has preconfigured rules from AWS that will offer comprehensive protection. Also allow the creation of customized rules.

AWS WAF is included at no additional cost for anyone who has purchased the AWS Shield Advanced tier.


### Exam TIP
To differentiate between Shield and WAF, it's important to note that Shield prevents DDoS attacks by operating at the Layer 3 and 4 network levels, while WAF analyzes the content of web traffic and requests at the Layer 7 level and can take specific actions based on that analysis.

## Networking and Content Delivery

### Virtual Private Cloud
Amazon VPC enable creating a logically defined space within AWS to create an isolated virtual network. Within this network, you retain full control over how the network is defined and allocated. You **fully control the IP space, subnets, routing tables, and network gateway** settings within your VPC, and you have full use of both IPv4 and IPv6.

You can have both public-facing and private network segments. 
- public: host applications like web systems
- private: host databases and other protected systems

#### Security Groups
Security Groups  Security groups in AWS are **virtual firewalls** that are used to **control inbound and outbound traffic**. 

Security groups are applied on the actual instance. This means that in a VPC where you have many services or virtual machines deployed, **each one can have different security groups applied to them**.

In fact, **each instance can have up to five security groups** applied to it, allowing different policies to be enforced.

Security groups that are created can **only be used within the VPC specified** when they were created.

- Security groups can have different rules for inbound and outbound traffic.
- By default, security groups allow all outbound traffic, but no inbound rules are applied by default.
- When specifying rules, you do them in the format of things allowed, not things denied.
- Security groups will automatically allow traffic that is in response to a request made by an allowed rule. For example, **if an inbound request is allowed to be made, the corresponding reply is allowed** to be made as well, regardless of what the rules specifically allow.

#### ACL
Access control lists (ACLs) are security layers on the VPC that **control traffic at the subnet level**. 

PS: ACL differs from security groups that are on each specific instance. 

- A VPC comes with a default ACL that allows all inbound and outbound traffic.
- Any custom ACLs will by default deny all inbound and outbound traffic until specific rules are applied to them.
- **Every subnet must have an ACL assigned** to it, either the default ACL or a custom one.
- Each **subnet may only have one ACL** attached to it, but **ACLs can be used for multiple subnets**.
- Differing from security groups, an ACL can have both allow and deny rules.
- Unlike security groups, **ACL rules do not automatically have state and allow responses to queries**. If an inbound request is made and allowed, an applicable outbound rule must be in place to allow the response to transmit.

#### Subnets
- **Subnets are very useful for segmenting your VPC**. You can use subnets to split up types of systems, public vs. private systems, or to apply different security groups. 


> For example, if you have an application that has public-facing web servers along with back-end application or database servers, you can place the two types of systems in separate subnets. That way, you can apply a security group to the public-facing servers to allow Internet access and a different security group to the application/database servers to only allow connections from the web servers, not from the public Internet. You can use routing tables to connect subnets across the AWS infrastructure for your particular needs.

- You can use routing tables to connect subnets across the AWS infrastructure for your particular needs.
- **Within a VPC, you must define a block of IP addresses that are available to it**. These are called a Classless Inter-Domain Routing (CIDR) block. 
- By default a VPC will be created with a CIDR of 172.31.0.0/16. 
- You can also choose to define your own CIDR block using the same notation as a traditional data center with block sizes between /16 and /28. 
- While the default block starts with 172.31, you can also specify your own desired block of addresses such as 10.10 or 192.168.

```
built-in firewalls: Security Group and Network ACLs
```

### Elastic Load Balancing
Elastic Load Balancing is used to distribute traffic across the AWS infrastructure. **This can be done on varying degrees of granularity, ranging from spanning across multiple Availability Zones or within a single Availability Zone**.

There are three different types of load balancing under its umbrella:
1. application load balancer, 
2. network load balancer, 
3. classic load balancer (Not Recommend to use).

#### Application Load Balancers ALB 
Application load balancing is most typically related to web traffic on both HTTP and HTTPS protocols. It has the ability to analyze the actual contents of the traffic and make determinations for routing and balancing based on rules.

Application load balancing **operates at Layer 7 of the OSI model**, which lends to the ability to scope load balancing rules that are very specific to each individual application and the underlying ways in which they are architected. Application load balancing within AWS operates within a VPC, so it can be used across hybrid cloud models as well.

TIP: Layer 7 of the OSI model pertains to the actual web traffic and content. Developers can take advantage of data such as the HTTP method, URL, parameters, headers, etc., in order to tune load balancing based on the specifics of their applications and the type of traffic and user queries it receives.

```
Can the ALB forward the response to the client using the same connection that was already established, or does it need to establish a new connection to send the response?

The Application Load Balancer (ALB) uses the same connection that was established between the client and the load balancer to forward the response from target servers back to the client[1][2]. This is possible because ALB supports HTTPS termination between clients and the load balancer, which means that the connection between your client and ALB is encrypted using SSL/TLS[2]. The ALB decrypts this traffic, forwards it to target servers as unencrypted HTTP traffic, receives the response from target servers, encrypts it using SSL/TLS, and forwards it back to the client over the same connection[1][2][3]. This allows for efficient handling of requests and responses without establishing new connections for each request/response cycle[1][2][4].

What do you mean by "termination"?

In the context of SSL/TLS, "termination" refers to the process of decrypting encrypted traffic at a load balancer or other network device and forwarding it as unencrypted traffic to target servers[1][2]. This allows target servers to handle unencrypted traffic and reduces the computational overhead required for decryption. The decrypted traffic is then re-encrypted before being sent back to the client over an encrypted connection[1][2]. SSL/TLS termination can be performed at various points in the network, including load balancers, reverse proxies, and other network devices[3][4][5].
```

```
Application Load Balancers (ALB) do not support mutual TLS authentication (mTLS) as they always terminate HTTPS connections[1]. Therefore, if you want to use mTLS with AWS, you need to create a TCP listener using a Network Load Balancer or a Classic Load Balancer and implement mTLS on the target servers[1]. You can set up your target servers with SSL/TLS server certificates (X.509 certificates), which are used for authentication and encryption during the SSL/TLS handshake process[2]. 
```

#### Network Load Balance
- Network load balancing is ideal for situations where performance is crucial and there may be high levels of traffic;
- It operates at the Layer 4 level of the OSI model, which is based on protocols and source/destination of traffic;
- It does not have the capabilities of ALB, where it can analyze the actual content of traffic and make decisions based on it;
- It also operates within a VPC, so it can bridge between different environments in a hybrid mode;

```
Network load balancing (NLB) is better equipped to handle sudden or volatile traffic patterns because it operates at the transport layer (Layer 4) of the OSI model, where it can distribute traffic based purely on protocols and the source and destination of traffic. This means that NLB can effectively distribute traffic across multiple servers without the overhead of processing and analyzing the actual application data, as is done in application load balancing. This makes NLB faster and more efficient in handling high volumes of traffic and sudden spikes, as it can more easily and quickly redirect traffic to the appropriate servers without causing delays or disruptions.
```
### Route 53
- Amazon Route 53 is a DNS service
- an organization can utilize Route 53 to transform names into their IP address
  - it can handle and transform names into their respective IPv6 addresses.
- Route 53 can be used for services that reside inside AWS, as well as those outside of AWS.
- Route 53 has the ability to configure health checks and monitor systems, enabling the routing of DNS queries to healthy systems or those with lower load.
- Route 53 DNS resolution can return different answers based on geographic location, current latency of systems, or pure round-robin .
### CloudFront
### Storage
### Elastic Block Store
### S3
### AWS Backup
### AWS Snow

## Compute Services
### EC2
### Elastic Beanstalk
### Lambda
### Containers


## Databases
## Automation
## End user computing