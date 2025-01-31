# VPC Fundamentals

* At the AWS Certified Developer level, the VPC skills that will be on the test are:
  * VPC, Subnets, Internet Gateways & NAT Gateways.
  * Security Groups, Network ACL (NACL), VPC Flow Logs.
  * VPC Peering, VPC Endpoint.
  * Site to Site VPN & Direct Connect.

### VPC & Subnets Primer

* VPC: private network to deploy your resources (regional resource).
* Subnets allow you to partition your network inside your VPC (Availability Zone resource).
* A public subnet is a subnet that is accessible from the internet.
* A private subnet is a subnet that is not accessible from the internet.
* To define access to the internet and between subnets, we use Route Tables.

**VPC Diagram**

<figure><img src="../../.gitbook/assets/image (199).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### Internet Gateway & NAT Gateways

* Internet Gateways helps our VPC instances connect with the internet.
* Public Subnets have a **route** to the internet gateway.
* • NAT Gateways (AWS-managed) & NAT Instances (self-managed) allow your instances in your Private Subnets to access the internet while remaining private.

<figure><img src="../../.gitbook/assets/image (200).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### Network ACL & Security Groups

* NACL (Network ACL):
  * A firewall which controls traffic from and to subnet.
  * Can have ALLOW and DENY rules.
  * **Are attached at the Subnet level.**
  * Rules only include IP addresses.
* Security Groups
  * A firewall that controls traffic to and from an ENI / an EC2 Instance.
  * Can have only ALLOW rules.
  * Rules include IP addresses and other security groups.

<figure><img src="../../.gitbook/assets/image (201).png" alt=""><figcaption><p>Font: AWS, 2023a</p></figcaption></figure>

### VPC Flow Logs

* Capture information about IP traffic going into your interface:
  * VPC Flow Logs.
  * Subnet Flow Logs.
  * Elastic Network Interface Flow Logs.
* Helps to monitor & troubleshoot connectivity issues. Example:
  * Subnets to internet.
  * Subnets to subnets.
  * Internet to Subnets.
* Captures network information from AWS managed interfaces too: Elastic Load Balancers, ElastiCachem RDS, Aurora, etc...
* VPC Flow logs data can go to S3, CloudWatch Logs, and Kinesis Data Firehose.

### VPC Peering

<figure><img src="../../.gitbook/assets/image (202).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* Connect two VPC, privately using AWS’ network.
* Make them behave as if they were in the same network.
* :warning:Must not have overlapping CIDR (IP address range).
* :warning:**VPC Peering connection is not transitive** (must be established for each VPC that need to communicate with one another).

### VPC Endpoints

* Endpoints allow you to **connect to AWS Services using a private network** instead of the public www network.
* This gives you enhanced security and lower latency to access AWS services.
* VPC Endpoint Gateway: S3 & DynamoDB.
* VPC Endpoint Interface: the rest.
* **Note: Only used within your VPC.**

<figure><img src="../../.gitbook/assets/image (203).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### Site to Site VPN & Direct Connect

* Site to Site VPN:
  * Connect an on-premises VPN to AWS.
  * The connection is automatically encrypted.
  * Goes over the public internet.
* Direct Connect (DX)
  * Establish a physical connection between on- premises and AWS.
  * The connection is private, secure and fast.
  * Goes over a private network.
  * Takes at least a month to establish.

<figure><img src="../../.gitbook/assets/image (204).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### Summary

* VPC: Virtual Private Cloud.
* Subnets: Tied to an AZ, network partition of the VPC.
* Internet Gateway: at the VPC level, provide Internet Access.
* NAT Gateway / Instances: give internet access to private subnets.
* NACL: Stateless, subnet rules for inbound and outbound.
* Security Groups: Stateful, operate at the EC2 instance level or ENI.
* VPC Peering: Connect two VPC with non overlapping IP ranges, non transitive.
* VPC Endpoints: Provide private access to AWS Services within VPC.
* VPC Flow Logs: network traffic logs.
* Site to Site VPN: VPN over public internet between on-premises DC and AWS.
* Direct Connect: direct private connection to a AWS.

### References

AWS. _Internetwork traffic privacy in Amazon VPC_. Amazon Web Services. Disponível em: [https://docs.aws.amazon.com/vpc/latest/userguide/VPC\_Security.html](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html). Acesso em: 17 set. 2023a.
