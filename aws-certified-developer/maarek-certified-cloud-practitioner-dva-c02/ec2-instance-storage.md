# EC2 Instance Storage

### Elastic Block Store- EBS

#### What is an EBS Volume?

* An EBS Volume is a durable, block-level storage device that can be attached to an EC2 instance.
* EBS volumes persist **independently from** the running life of an **EC2** instance, and when you create an EBS volume.
* It is **automatically** **replicated** within its **Availability Zone** to prevent data loss due to failure of any single hardware component.
* EBS volumes are particularly **well-suited** for use as the primary storage **for data that requires frequent updates**, such as the system drive for an instance or storage for a database application.

> Think of them as a "network USB stick" (MAAREK, 2023).

* It is a network drive (i.e. not a physical drive):
  * It uses the network to communicate the instance, which means there might be a bit of latency.
  * It can be detached from an EC2 instance and attached to another one quickly.
* Have a provisioned capacity (size in GBs and IOPS).
  * You get billed for all the provisioned capacity.
  * You can increase capacity of the drive over time.

#### EBS - Delete on Termination Attribuite

<figure><img src="../../.gitbook/assets/image (141).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* Controls the EBS behaviour when an EC2 instance terminates:
  * By default, the root EBS volume is deleted (attribute enabled).
  * By default, any other attached EBS volume is not deleted (attribute disabled).
* **\[Exam Question] Use case: preserve root volume when instance is terminated.**

#### **EBS Volume Types**

* OBS: IOPS stands for input/output operations per second.
* EBS Volumes come in 6 types:
  * _gp2/gp3 (SSD)_: General purpose (gp) SSD volume that balances price and performance.
  * _io1/io2 (SSD)_: Highest-performance SSD volume for mission-critical low latency or high-throughput workloads.
  * _st1 (HDD)_: Low cost HDD volume designed for frequently accessed, throughput-intense workloads.
  * _sc1 (HDD)_: Lowest cost HDD volume designed for less frequently accessed workloads.
* EBS Volumes are characterised in **Size | Throughput | IOPS (I/O Ops Per Sec).**
* Only gp2/gp3 and io1/io2 can be used as boot volumes.

#### General Purpose SSD (gp2/gp3)

* Cost effective storage, low-latency
* System boot volumes, virtual desktops, development and test environments.
* Capacity: 1GiB - 16TiB.
* gp3:
  * Newest generation so far.
  * Baseline of 3000 IOPS and throughput of 125 MiB/s.
  * Can increase IOPS up to 16000 and throughput up to 1000Mi/s _**independently**_ :exclamation::exclamation:
* _gp2:_
  * Small gp2 volumes can burst IOPS to 3000.
  * _**Size of the volume and IOPS are linked**_:exclamation:, max IOPS is 16000
  * 3 IOPS per GB, means at 5,334GB we are at the MAX IOPS.

#### Hard Disk Drivers

* Cannot be a boot volume.
* 125 GiB to 16TiB.
* Throughput Optimized HDD (_**st1**_):
  * Big Data, Data Warehouses, Log Processing.
  * Max Throughput 500 MiB/s - max IOPS 500.
* Cold HDD (_**sc1**_):
  * For data that is infrequently accessed.
  * Scenarios where lowest cost is importand.
  * Max Throughput 250 MiB/s - max IOPS 250.

#### EBS - Volume Types Summary

<figure><img src="../../.gitbook/assets/image (131).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Provisioned IOPS (PIOPS) SSD (io1/io2)

* Critical business applications with sustained IOPS performance.
* Or applications that need more than 16000 IOPS.
* Great for **databases workloads.**
* io1/io2 (4GiB - 16TiB):
  * Max PIOPS: 64000 for Nitro EC2 instances & 32000 for others.
    * OBS: _**Nitro-based EC2 instances**_ are the next generation of EC2 instances that use the AWS Nitro System, a virtualization infrastructure that offloads and accelerates I/O for functions such as VPC, EBS, and ENA.
  * **Can increase PIOPS independently from storage size**:exclamation:
  * io2 have more durability and more IOPS per GiB (at the same price as io2).
* Io2 Block Express (4GiB - 64 TiB):
  * Sub-milisecond latency.
  * Max PIOPS: 256000 with an IOPS:GiB ratio of 1000:1
* **Supports EBS Multi-attach** :exclamation:

### EBS Snapshots

* Make a backup (snapshot) of your EBS volume at a point in time.
* Not Necessary to detach volume to do snapshot, but recommended.
* Can Copy snapshots across AZ or Region.

<figure><img src="../../.gitbook/assets/image (142).png" alt="" width="557"><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### EBS Snapshots Features

* EBS Snapshot Archive:
  * Move a snapshot to an "archive tier" that is 75% cheaper.
  * Takes within 24 to 72 hours for restoring the archive.
* Recycle Bin for EBS Snapshots:
  * Setup rules to retain deleted snapshots so you can recover them after an accidental deletion.
* Fast Snapshot Restore (FSR):
  * Force full initialization of snapshot to have no latency on the first use (\$$\$$$).

### EBS Multi-Attach (`io1` and `io2` family)

* Attach the same EBS volume to multiple EC2 instances _**in the same AZ**_**.**
* Each instance has full read & write permissions to the high-performance volume.
* **Use case:**
  * Achieve higher application availability in clustered Linux applications (ex: Teradata).
  * Applications must manage concurrent write operations.
* **\[Exam Question]** **UP TO 16 EC2 INSTANCES AT A TIME**
* Must use a file system that's cluster aware (not XFS, EXT4, etc).

### AMI Overview

* AMI = Amazon Machine Image.
* AMI are a customization of an EC2 instance.
  * You add your own software, configuration, O.S., etc...
  * Faster boot/ configuration time because all your software is pre-package.
* **AMI are built for a specific region** (and can be copied across regions).
* EC2 instances can be launched from:
  * Public AMI: AWS provided.
  * Your own AMI.
  * AWS Marketplace.

### EC2 Instance Store

* EBS volumes are network drives with good but "limited" performance.
* If you need a even high-performance hardware disk, use **EC2 Instance Store.**
  * Better I/O performance.
* EC2 instance Store lose their storage if they are stopped (:warning:**ephemeral**:warning: ).
  * Risk of data loss if hardware fails.
  * OBS: Backups and replication are customer responsibility.
* Use case:
  * Good for buffer, cache, scratch data, temporary content.

### Amazon EFS - Elastic File System

* Managed Network File System (NFS) that can be mounted on many EC2.
* EFS works with EC2 instances in multi-AZ.
* Highly available, scalable, expensive (3x gp2), **pay per use**.

<figure><img src="../../.gitbook/assets/image (132).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* **Use cases:**
  * content management.
  * web serving.
  * data sharing.
  * wordpress.
* Uses NFS4.1 protocol.
* Uses security group to control access to EFS.
* Compatible with Linux based AMI (NOT Windows).
* Encryption at rest using KMS.
* POSIX file system (\~Linux) that has a standard file API.
* **NO CAPACITY PLANNING!** File system scales automatically, pay-per-use (per GiB).

#### EFS - Performance

* EFS Scale:
  * 1000s of concurrent NFS clients, 10 GB+ of throughput.
  * Grow to petabyte-scale network file system, automatically.
* Performance Mode (set at EFS creation time).
  * General purpose (default) - **latency-sensitive** use cases (web server, CMS, etc...).
  * Max I/O - higher latency, throughput, highly parallel (big data, media processing).
* Throughput Mode
  * Bursting: 1 TB = 50MiB/s + burst of up to 100MiB/s
  * Provisioned: set your throughput regardless of storage size, ex: 1 GB/s for 1 TB storage.
  * Elastic: Automatically scales throughput up or down based on your worklods.
    * Up to 3GB/s for reads and 1GB/s for writes.
    * Used for unpredictable workloads.

#### EFS - Storage Classes

*   Storage Tiers (life-cycle management feature - move file after N days).

    * Standard: for frequently accessed files.
    * Infrequent access (EFS-IA): cost to retrieve files, lower price to store. Enable EFS-IA with a Life-cycle policy

    <figure><img src="../../.gitbook/assets/image (133).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>
* Availability and durability
  * Standard: Multi-AZ, great for prod.
  * One Zone: One AZ, great for dev
    * backup enabled by default, compatible with IA (infrequent access) (EFS One Zone-IA).

#### What the Exam usually asks about EFS?

> The exam will ask you when should you use EFS and then what options shoud you set on your EFS to ensure that you validade and comply with the requirements.

### Questions

You are running a high-performance database that requires an IOPS of 310,000 for its underlying storage. What do you recommend?

A. Use an Ec2 instance Store

B. Use an EBS gp2 drive

C. Use an EBS io1 drive

D. Use an EBS io2 Block Express Drive

<details>

<summary>Answer</summary>

A

You can run a database on an EC2 instance that uses an Instance Store, but you'll have a problem that the data will be lost if the EC2 instance is stopped (it can be restarted without problems). One solution is that you can set up a replication mechanism on another EC2 instance with an Instance Store to have a standby copy. Another solution is to set up backup mechanisms for your data. It's all up to you how you want to set up your architecture to validate your requirements. In this use case, it's around IOPS, so we have to choose an EC2 Instance Store.

</details>

You have provisioned an 8TB `gp2` EBS volume and you are running out of IOPS. What is **NOT** a way to increase performance?

A. A Mount EBS volumes in RAID 0

B. Change to an io1 volume type

C. Increase the EBS volume size

<details>

<summary>Answer</summary>

C

For EBS gp2 volumes, it has max. IOPS of 16,000 or equivalent 5334 GB.

</details>
