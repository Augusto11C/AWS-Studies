# Secrets Manager

* **AWS Secrets Manager**: This service is designed for securely storing and managing sensitive information like API keys, passwords, and database credentials. It’s different from the SSM Parameter Store in that it offers advanced features for secret management.
* **Secret Rotation**: One of the key features of Secrets Manager is the ability to automatically rotate secrets on a schedule (every X number of days). This helps to maintain a strong security posture by reducing the potential impact if a secret is compromised.
* **Automated Secret Generation**: Within Secrets Manager, you can automate the generation of new secrets during rotation. This typically involves creating a Lambda function that generates new secrets, enhancing security and reducing manual effort.
* **Integration with AWS Services**: Secrets Manager is well integrated with various AWS services. For example, it works seamlessly with Amazon RDS for MySQL, PostgreSQL, SQL Server, and Aurora. But its integration isn’t limited to these; it works with other AWS services and databases as well.
* **Secret Storage**: The username and password needed to access your database can be securely stored directly in Secrets Manager. These can be rotated regularly, ensuring your database credentials are not static and therefore less vulnerable.
* **Encryption**: Secrets stored in Secrets Manager can be encrypted using the AWS Key Management Service (KMS), providing an additional layer of security.
* **Multi-region Secrets**: Another feature of Secrets Manager is the ability to replicate secrets across multiple AWS regions. This means if you create a secret in one region (the primary region), it gets replicated as the same secret into another region (the secondary region). This is useful for building multi-region applications, implementing disaster recovery strategies, or if you have an RDS database that is also being replicated from one region to another. In case of a problem with one region (e.g., US East 1), you can promote a replica secret as a standalone secret in another region.

In summary, AWS Secrets Manager provides robust capabilities for managing secrets in a secure and automated manner, making it an essential tool for maintaining the security of your applications.

### Secrets Manager - CloudFormation Integration

**AWS Secrets Manager (Premium Service)**:

* Offers automatic rotation of secrets using AWS Lambda, enhancing security by ensuring secrets are not static.
* Provides a ready-to-use Lambda function for services like RDS, Redshift, and DocumentDB, simplifying the process of secret rotation.
* Requires encryption using AWS Key Management Service (KMS), ensuring that all secrets are securely stored.
* Fully integrates with AWS CloudFormation, allowing you to manage your secrets alongside your infrastructure.

**AWS Systems Manager (SSM) Parameter Store (Cost-effective option)**:

* Provides a simple API for storing and retrieving parameters, making it easy to use.
* Does not offer built-in secret rotation. However, you can set up rotation using a Lambda function triggered by CloudWatch Events.
* Encryption using AWS KMS is optional, giving you flexibility based on your security needs.
* Also integrates with AWS CloudFormation, allowing you to manage your parameters alongside your infrastructure.
* Can retrieve secrets stored in Secrets Manager using the SSM Parameter Store API, providing a bridge between the two services.

Both services offer secure storage for sensitive information. Secrets Manager provides more advanced features like automatic secret rotation, while SSM Parameter Store is a simpler and more cost-effective option. The choice between the two depends on your specific needs and budget.

<figure><img src="../../.gitbook/assets/image (268).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>
