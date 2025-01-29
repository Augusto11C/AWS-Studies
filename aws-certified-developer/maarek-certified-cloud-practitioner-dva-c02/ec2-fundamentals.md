# EC2 Fundamentals

* EC2 = Elastic Compute Cloud = Infrastructure as a Service
* It mainly consists in the capability of :
  * Renting virtual machines (EC2).
  * Storing data on virtual drives (EBS).
  * Distributing load across machines (ELB).
  * Scaling the services using an auto-scaling group (ASG).

### EC2 Sizing and Configuration Options

* OS: Linux, Windows or Mac OS.
* How much CPU.
* How much RAM.
* How Much storage space:
  * Network-attached (EBS & EFS).
  * Hardware (EC2 Instance Store).
* Network card: speed of the card, Public IP.
* Firewall rules: **security group.**
* Bootstrap script (configure at first launch).

### EC2 User Data

* It is possible to bootstrap our instances using an EC2 User data script.
* Bootstrapping means launching commands when a machine starts.
* That script in only run once at the instance first start.
* EC2 user data is used to automate boot tasks such as:
  * Installing updates.
  * Installing software.
  * Downloading common files from the internet.
  * Etc.
* The Ec2 User Data Script runs with the **root user.**

### Security Groups

* Security Groups are the fundamental of network security in AWS.
* They act as a "firewall".
*   They control how traffic is allowed into or out of our EC2 Instances.

    <figure><img src="../../.gitbook/assets/image (149).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>
* Security groups only contain **allow** rules.
* Security groups rules can reference by IP or by security group.

**What does Security Group regulate?**

* Access to Ports.
* Authorised IP ranges.
* Control of inbound network (from other to the instance).
* Control of outbound network (from the instance to other).
*   AWS Console example:

    <figure><img src="../../.gitbook/assets/image (150).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Good to Know About Security Groups

* Can be attached to multiple instances.
* Locked down to a Region/VPC combination (need to recreate securitu group of you change the region or VPC).
* Does live "outside" the EC2 - if traffic is blocked the EC2 won't see it.
* It's good to maintain one separate security group for SSH access.
* If your application is not accessible (time out), then it's a security group issue.
* If your application gives a "connection refused" error, then it's an application error or it's not launched.
* All inbound traffic is **blocked** by default.
* All outbound traffic is **authorised** by default.

#### <mark style="background-color:yellow;">Classic ports to know</mark>

* 22 = SSH (Secure Shell) - Log into a Linux Instance.
* 21 = FTP (File Transfer Protocol) - Upload files into a file share.
* 22 = SFTP (Secure File Transfer protocol) - Upload Files using SSH.
* 80 = HTTP - access unsecured websites.
* 443 = HTTPS - access secured websites.
* 3389 = (Remote Desktop Protocol) - Log into a windows instance.

### EC2 Instances Purchasing Options

1. **On-demand instances:** short workload, predictable pricing, pay by second.
2. **Reserved (1 & 3 years):**
   * Reserved instances - long workloads.
   * Convertible Reserved Instances - long workloads with flexible instances.
3. **Savings Plans (1 & 3 years)** - commitment to an amount of usage, long workload.
4. **Spot Instances** - short workloads, cheap, can lose instances (less reliable).
5. **Dedicated Hosts** - book an entire physical server, control instance placement.
6. **Dedicated Instances** - no other customers will share your hardware.
7. **Capacity Reservations** - Reserve capacity in a specific AZ for any duration.

#### EC2 Reserved Instances

* Up to **72%** discount compared to On-demand.
* You reserve a specific instance attributes (instance type, region, tenancy, OS).
* Reservation Period - 1 yeat (+discount) or 3 years (+++discount).
* Payment options - No upfront (+), partial upfront(++), all upfront (+++).
* Reserved Instance's Scope - Regional or Zonal (Reserve capacity in an AZ).
* Recommended for steady-state usage applications (e.g. database).
* It is possible to buy or sell reserved instance marketplace.
* **Convertible reserved instance:**
  * Can change the EC2 instance type, instance family, OS, scope and tenancy.
  * Up to **66%** discount.

#### EC2 Saving Plans

* Get a discount based on long-term usage (up to 72% \~ same as RIs).
* Commit to a certain type of usage ($10/hour for 1 or 3 years):
  * "I want to spend $10 per hour for the next 3 years".
  * Usage beyond EC2 Saving Plans is billed at the On-demand price.
* Locked to a specific instance family & AWS Region (e.g. m5 in us-easy-1).
* Flexible across:
  * instance Size (e.g. m5.xlarge, m5.**2**xlarge).
  * OS (e.g. Linux Windows).
  * Tenancy (e.g. Host, Dedicated, Default).

#### EC2 Spot Instances

* Up to 90% discount compared to On-demand.
* Instances that you can "lose" at any point of time if your max price is less than the current spot price.
* The MOST cost-efficient instances in AWS
* Useful for workloads that are **resilient:**
  * Batch jobs
  * Data analysis
  * Image processing
  * Any distributed workloads
  * Workloads with flexible start and end

**EC2 Capacity Reservations**

* Reserve On-demand instances capacity in a specific AZ for any duration.
* You always have access to EC2 capacity when you need it.
* No time commitment, no billing discounts.
* Combine with Regional Reserved Instances and Savings Plans to Benefit from billing discounts.
* You are charged at On-demand rate whether you run instances or not.
* **Suitable for short-term, uninterrupted workloads that needs to be in a specific AZ.**

#### EC2 Dedicated Hosts

* A physical server with EC2 instance capacity fully dedicated to your use.
* Allows you address compliance requirements and use your existing **server-bound software licenes (per socket, per core, per VM software licenses).**
* Purchasing Options:
  * On-demand – pay per second for active Dedicated Host.
  * Reserved - 1 or 3 years (No Upfront, Partial Upfront, All Upfront).
* The most expensive option.
* **Useful for software that have complicated licensing model (BYOL – Bring Your Own License).**
* Or for companies that have strong regulatory or compliance needs.

#### EC2 Dedicated Instances

* Instances run on HW that is dedicated to you.
* May share HW with other instances in same account.
* No control over instance placement.

#### EC2 Dedicated instances Vs. EC2 Dedicated Host

**EC2 Dedicated Instances:**

* EC2 instances that run on hardware that's dedicated to a single customer.
* Can be launched into a VPC with a tenancy attribute of "dedicated".
* Instances run on single-tenant hardware.
* Can be used to launch instances of varying sizes that belong to a single instance type.
* The number of instances is limited to the number of cores available on the host.
* Can be shared among multiple organizations or accounts.
* No performance, security, or physical differences between Dedicated Instances and Dedicated Hosts.

**EC2 Dedicated Hosts:**

* A physical server fully dedicated for your use.
* Provides Affinity that allows you to specify which instances launch onto which host.
* Provides visibility and the option to control how you place your instances on a specific, physical server.
* Enables you to deploy instances using configurations that help address corporate compliance and regulatory requirements.
* Automatic host maintenance with scheduling control.
* Can be used to launch instances of varying sizes that belong to a single instance type.
* Software licenses can be used on Dedicated Hosts, which is not possible with Dedicated Instances.

> Dedicated instances mean that you have your own instance on your own hardware, wheareas dedicated host, you get access to the physical server itself and it gives you visibility into the lower level hardware.
