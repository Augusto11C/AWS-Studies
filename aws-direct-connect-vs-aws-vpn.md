# AWS Direct Connect vs AWS VPN

Instances that you launch into a VPC can't communicate with your own (remote) network by default.

AWS Direct Connect and VPN are two different connectivity options for businesses to connect their on-premises infrastructure to AWS cloud services.

### AWS Direct Connect

* Provides a dedicated, private network connection between the on-premises infrastructure and AWS cloud services.
* Offers higher security as there is no exposure to the public internet.
* Provides greater bandwith than an internet-based VPN.
* Is suitable for larger businesses or organisations with higher data transfer needs.
* Is more expensive than VPN.

<figure><img src=".gitbook/assets/image (135).png" alt=""><figcaption><p>SHANNON, 2023</p></figcaption></figure>

### AWS VPN

* Utilizes the public internet to establish an encrypted network connection between the on-premises infrastructure and AWS cloud services.
* Has lower network performance and security than direct Connect.
* Is suitable for businesses that are just getting started with AWS and have a low to moderate bandwidth requirements.

**How connection using VPN can occur?**

* Attach a virtual private gateway to the VPC.
* Create a custom route table.
* Update the security group rules.
* Create an AWS Managed VPN Connection.

<figure><img src=".gitbook/assets/image (134).png" alt=""><figcaption><p>SHANNON, 2023</p></figcaption></figure>

### Reference

SHANNON, Michael. AWS Cloud Practitioner. O'Reilly: Live event, 2023.
