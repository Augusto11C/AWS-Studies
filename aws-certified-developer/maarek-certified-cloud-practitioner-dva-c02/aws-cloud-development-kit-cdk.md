# AWS Cloud Development Kit (CDK)

<figure><img src="../../.gitbook/assets/image (334).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* **What is AWS CDK?** AWS CDK allows you to define your cloud infrastructure in a familiar programming language such as JavaScript, TypeScript, Python, Java, or .NET. It’s a tool that supersedes CloudFormation.
* **How does it work?** You define your infrastructure using constructs, which are high-level components provided by the CDK. For example, you can define a VPC, an ECS cluster, and an application load-balanced Fargate service using TypeScript. If the code compiles successfully, it is then synthesized into a CloudFormation template in either JSON or YAML format.
* **Benefits:** AWS CDK allows you to deploy infrastructure and application runtime code together. This is particularly useful for Lambda functions and Docker containers in ECS or EKS. It also provides type safety, which means you’ll know at compile time if your template has errors.
* **Difference between CDK and SAM:** While both tools leverage CloudFormation in the backend, they serve different purposes. SAM (Serverless Application Model) is focused on serverless applications and uses declarative JSON or YAML templates. It’s great for quickly getting started with Lambda. On the other hand, CDK supports all AWS services and allows you to write your infrastructure using a programming language you know.
* **Integration with SAM:** You can use the SAM CLI to locally test your CDK applications. To do this, you first run `cdk synth` to synthesize your application into a CloudFormation template.
