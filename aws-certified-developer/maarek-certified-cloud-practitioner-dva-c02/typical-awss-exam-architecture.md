# Typical AWS's Exam Architecture

#### Three Tier Solution Architecture

<figure><img src="../../.gitbook/assets/image (205).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

The 3 Tier Architecture is comprised of three (3)distinct layers. A user accesses an ELB hosted in a public subnet and this interacts with EC2s (Backend) in a private subnet which saves and retrieves information from the Database in the data subnet.

#### More Three Tier Architectures (To Review)

* [Creating Highly Available 3-Tier Architecture in AWS](https://blog.devgenius.io/creating-highly-available-3-tier-architecture-in-aws-ba429dbffcda)
* [Creation of a Highly Available 3 Tier Architecture](https://aws.plainenglish.io/creation-of-a-highly-available-3-tier-architecture-30b2be0a871e)
* [AWS 3-Tier Architecture](https://awstip.com/aws-3-tier-architecture-4b0004960f4)
* [Deploying the full three-tier application](https://dev.to/eelayoubi/building-a-ha-aws-architecture-using-terraform-part-2-30gm)

### LAMP Stack on EC2

* Linux: OS for EC2 instances.
* Apache: Web Server that run on Linux (EC2).
* MySQL: database on RDS.
* PHP: Application logic (running on EC2).
* Extra features:
  * Can add Redis / Memcached (ElastiCache) to include a caching tech.
  * To store local application data & software: EBS drive (root).

### Wordpress on AWS

<figure><img src="../../.gitbook/assets/image (207).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (206).png" alt=""><figcaption><p>Font: LEWIS, 2023</p></figcaption></figure>

### AWS Architecture Center

* [AWS Architecture Use Cases](https://aws.amazon.com/architecture/reference-architecture-diagrams/?solutions-all.sort-by=item.additionalFields.sortDate\&solutions-all.sort-order=desc\&whitepapers-main.sort-by=item.additionalFields.sortDate\&whitepapers-main.sort-order=desc\&awsf.whitepapers-tech-category=*all\&awsf.whitepapers-industries=*all).
* [Top 5: Featured Architecture Content from December 2021](https://aws.amazon.com/blogs/architecture/top-5-featured-architecture-content-for-november/).
* [Top 5: Featured Architecture Content for November 2021](https://aws.amazon.com/blogs/architecture/top-5-featured-architecture-content-for-november/).
* [AWS Architecture Blog Category: Financial Services](https://aws.amazon.com/blogs/architecture/category/industries/financial-services/).
* [AWS Architecture Blog Category: Best Practices](https://aws.amazon.com/blogs/architecture/category/post-types/best-practices/).

### References

* LEWIS, Paul. WordPress: Best Practices on AWS. AWS Architecture Blog, 2018. Disponível em: <[https://docs.aws.amazon.com/vpc/latest/userguide/VPC\_Security.html](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html)>. Acesso em: 17 set. 2023.
