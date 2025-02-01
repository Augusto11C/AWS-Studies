# SSM Parameter Store

* **AWS Systems Manager Parameter Store (SSM Parameter Store)**: This is a secure, scalable, and durable service that provides centralized storage for your configuration data and secrets. It’s serverless, meaning you don’t need to manage any underlying infrastructure.
* **Encryption**: You have the option to encrypt your configurations using the AWS Key Management Service (KMS), turning them into secure secrets. This adds an extra layer of security for sensitive information.
* **Ease of Use**: The SDK for SSM Parameter Store is user-friendly, making it easy to interact with the service programmatically.
* **Version Tracking**: Whenever you update your parameters, SSM Parameter Store keeps track of the different versions. This allows you to manage changes and roll back if necessary.
* **Security**: Access to parameters is controlled through AWS Identity and Access Management (IAM), ensuring that only authorized entities can access your data.
* **Notifications**: You can receive notifications about parameter changes through Amazon EventBridge, helping you stay informed about important events.
* **Integration with CloudFormation**: SSM Parameter Store is fully integrated with AWS CloudFormation. This means that you can use parameters from the Parameter Store as input for your CloudFormation stacks, simplifying your infrastructure management process.
