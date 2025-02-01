# Route 53

### DNS Terminologies

* Domain Registrar: Amazon Route 53, GoDaddy, …
* DNS Records: A, AAAA, CNAME, NS, …
* Zone File: contains DNS records
* Name Server: resolves DNS queries (Authoritative or Non-Authoritative)
* Top Level Domain (TLD): .com, .us, .in, .gov, .org, …
* Second Level Domain (SLD): amazon.com, google.com, …

### Route 53

#### Route 53 - Record Types

* A: Maps a hostname to IPv4.
* AAAA: Maps a hostname to IPv6.
* CNAME: maps a host name to another hostname.
  * The target is a domain name which must have an A or AAAA record.
  * Can't create a CNAME record for the top node of a DNS namespace (Zone Apex).
  * Example: you cannot create for `example.com`, but you can create for `www.example.com`.
* NS - Name servers for the Hosted Zone.
  * Control how traffic is routed for a domain.

#### Route 53 - Hosted Zones

* A container for records that define how to route traffic to a domain and its subdomains.
* **Public Hosted Zones:** Contains **records** that specify **how to route traffic on the internet** (public domain names).
* Private Hosted Zones: Contains records that specify how you route traffic within one or more VPCs (private domain names).
* Note: You pay $0,50 per month per hosted zone.

<figure><img src="../../.gitbook/assets/image (175).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Route 53 - Records TTL

* High TTL – e.g., 24 hr.
  * Less traffic on Route 53.
  * Possibly outdated records.
* Low TTL – e.g., 60 sec.
  * More traffic on Route 53 (\$$).
  * Records are outdated for less time.
  * Easy to change records.
* Note: Except for alias records, TTL is mandatory for each DNS record.

<figure><img src="../../.gitbook/assets/image (92).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Route 53 - CNAME vs Alias

* AWS Resources (Load Balancer, CloudFront...) expose an AWS hostname:
  * `lb1-1234.us-east-2.elb.amazonaws.com` and you want `myapp.mydomain.com`.
* CNAME:
  * Points a hostname to any other hostname.
    * Example: `app.mydomain.com` -> `blabla.anything.com`.
    * Only for Non Root Domains (aka. `something.mydomain.com`).
* Alias:
  * Points a hostname to an AWS Resource (`app.mydomain.com` -> `blabla.amazonaws.com`).
  * Works for Root Domain and Non Root Domain (aka. `mydomain.com`).
  * Free of charge.
  * Native Healthcheck.

#### Route 53 - Alias

* Maps a hostname to an AWS resource.
* An extesion to DNS functionality.
* Automatically recognizes changes in the resource's IP adresses.
* Unlike CNAME, it can be used for the top node of a DNS namespace (Zone Apex), e.g.: `example.com`.
* Alias record is always of type A, AAAA for AWS resources (IPv4/IPv6).
* Note: You can't set the TTL.
* Alias records Targets:
  * Elastic load Balancers.
  * CloudFront Distributions.
  * API Gateway.
  * Elastic Beanstalk Environments.
  * S3 Websites.
  * VPC Interface Endpoints.
  * Global Accelerator accelerator.
  * Route 53 record in the some hosted zone.
* Note: You cannot set an ALIAS record for an EC2 DNS name.

### Route 53 - Routing Policies

* Define how Route 53 responds to DNS queries.
* Don’t get confused by the word “Routing”.
  * It’s not the same as Load balancer routing which routes the traffic.
  * DNS does not route any traffic, it only responds to the DNS queries.
* • Route 53 Supports the following Routing Policies:
  * Simple
  * Weighted
  * Failover
  * Latency based
  * Geolocation
  * Multi-Value Answer
  * Geoproximity (using Route 53 Traffic Flow feature)

#### Route 53 - Simple Routing Policy

* Typically, route traffic to a single resource.
* Can specify multiple values in the same record.
* If multiple values are returned, a random one is chosen by the client.
* When Alias enabled, specify only one AWS resource.
* :warning::exclamation:Can’t be associated with Health Checks.
  * You can use Multi-Value Routing Policy

#### Route 53 - Weighted Routing Policy

* Control the % of the requests that go to each specific resource.
* Assign each record a relative weight:
  * ![](<../../.gitbook/assets/image (176).png>)
  * Weights don’t need to sum up to 100.
* DNS records must have the same name and type.
* Can be associated with Health Checks
* Use Cases:
  * Load Balancing between regions.
  * testing new application versions.
* Assign a weight of 0 to a record to stop sending traffic to a resource.
* If all records have weight of 0, then all records will be returned equally.

<figure><img src="../../.gitbook/assets/image (177).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Route 53 - Weighted Routing Policy

* Redirect to the resource that has the least latency close to us.
* :warning:Super helpful when latency for users is a priority.
* **Latency is based on traffic between users and AWS Regions.**
* Can be associated with Health Checks (has a failover capability)

#### Route 53 - Failover (Active-Passive) Routing Policy

* Amazon Route 53’s Failover (Active-Passive) Routing Policy is designed to achieve high availability by routing traffic to a primary resource or group of resources, and switching to a secondary resource or group of resources in case of a failure.
* Amazon Route 53 includes only the healthy primary resources3. **If all the primary resources are unhealthy**, Route 53 begins to **include only the healthy secondary resources** in response to DNS queries.

<figure><img src="../../.gitbook/assets/image (181).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

**Console Config:**

<figure><img src="../../.gitbook/assets/image (182).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (184).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

Create a simple health check for the endpoint and it will work. If the primary record (1) fails, the result returned to the DNS will be record (2).

#### Route 53 - Geolocation Routing Policy

* This routing is based on user location.
* Specify location by Continent, Country or by US State (if there’s overlapping, most precise location selected).
* :warning: Should create a “Default” record (in case there’s no match on location).
* Use cases: website localization, restrict content distribution, load balancing, …
* Can be associated with Health Checks.

<figure><img src="../../.gitbook/assets/image (185).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Route 53 - Geoproximity Routing Policy

* Route traffic to your resources based on the geographic location of users and resources.
* Ability to s**hift more traffic to resources** based on the defined **bias**.
* To change the size of the geographic region, specify bias values:
  * To expand (1 to 99) – more traffic to the resource.
  * To shrink (-1 to -99) – less traffic to the resource.
* Resources can be:
  * AWS resources (specify AWS region)
  * Non-AWS resources (specify Latitude and Longitude)
* • You must use Route 53 Traffic Flow to use this feature

**Diagram Example:**

<figure><img src="../../.gitbook/assets/image (189).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (190).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Route 53 - Traffic Flow **Console Config** Geoproximity Routing Policy

* Simplify the process of creating and maintaining records in large and complex configurations.
* Visual editor to manage complex routing decision trees.
* Configurations can be saved as Traffic Flow Policy:
  * Can be applied to different Route 53 Hosted Zones (different domain names).
  * Supports versioning.

<figure><img src="../../.gitbook/assets/image (186).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (187).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (188).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Route 53 - Geolocation Routing Policy

* Routing is based on clients’ IP addresses.
* You provide a list of CIDRs for your clients and the corresponding endpoints/locations (user-IP-to-endpoint mappings).
* **Use cases:** Optimize performance, reduce network costs…
* **Example:** route end users from a particular ISP to a specific endpoint.

<figure><img src="../../.gitbook/assets/image (191).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Route 53 - Multi-Value Routing Policy

* Use when routing traffic to multiple resources.
* Route 53 return multiple values/resources.
* :warning: Can be associated with Health Checks (return only values for healthy resources).
* Up to 8 healthy records are returned for each Multi-Value query.
* :warning:Multi-Value is not a substitute for having an ELB.

**Scenario When All Endpoints are Healthy**

<figure><img src="../../.gitbook/assets/image (195).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (192).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (194).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

**Scenario When There is an unheathy endpoint**

<figure><img src="../../.gitbook/assets/image (196).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (197).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### Route 53 - Health Checks

* HTTP Health Checks are only for public resources.
* Health Check => Automated DNS Failover:
  * Health checks that monitor an endpoint (application, server, other AWS resource).
  * Health checks that monitor other health checks (Calculated Health Checks).
  * Health checks that monitor CloudWatch Alarms (full control !!) – e.g., throttles of DynamoDB, alarms on RDS, custom metrics, … ( :warning: **helpful for private resources**)

#### Health Cheks - Monitor an Endpoint

* About 15 global health checkers will check the endpoint health:
  * Healthy/Unhealthy Threshold – 3 (default).
  * Interval – 30 sec (can set to 10 sec – :moneybag:higher cost).
  * Supported protocol: HTTP, HTTPS and TCP.
  * **If > 18%** of health checkers report the endpoint is healthy, **Route 53 considers it Healthy**. Otherwise, it’s Unhealthy.
  * Ability to choose which locations you want Route 53 to use.
* Health Checks pass only when the endpoint responds with the 2xx and 3xx status codes.
* Health Checks can be setup to pass / fail based on the text in the first 5120 bytes of the response.
* :warning:]Configure you router/firewall to allow incoming requests from Route 53 Health Checkers.

<figure><img src="../../.gitbook/assets/image (178).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Health Cheks - Calculated Health Checks

* Combine the results of multiple Health Checks into a single Health Check.
* You can use OR, AND, or NOT.
* Can monitor up to 256 Child Health Checks.
* Specify how many of the health checks need to pass to make the parent pass.
* Usage: perform maintenance to your website without causing all health checks to fail.

<figure><img src="../../.gitbook/assets/image (179).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Health Checks - Private Hosted Zones

* :warning:Route 53 health checkers are outside the VPC.
* :warning: They can’t access private endpoints (private VPC or on-premises resource).
* You can **create a CloudWatch Metric and associate a CloudWatch Alarm**, then create a **Health Check** that **checks the alarm** itself.

<figure><img src="../../.gitbook/assets/image (180).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### Domain Registar vs. DNS Service

* • You buy or register your domain name with a Domain Registrar typically by paying annual charges (e.g., GoDaddy, Amazon Registrar Inc., …).
* The Domain Registrar usually provides you with a DNS service to manage your DNS records.
* But you can use another DNS service to manage your DNS records.
* Example: purchase the domain from GoDaddy and use Route 53 to manage your DNS records.

<figure><img src="../../.gitbook/assets/image (198).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* • If you buy your domain on a 3rd party registrar, you can still use Route 53 as the DNS Service provider:
  * (1) Create a Hosted Zone in Route 53.
  * (2) Update NS Records on 3rd party website to use Route 53 Name Servers.
* :exclamation::warning: Domain Registrar != DNS Service.
* But every Domain Registrar usually comes with some DNS features.
