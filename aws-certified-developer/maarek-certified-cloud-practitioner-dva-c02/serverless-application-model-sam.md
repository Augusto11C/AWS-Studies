# Serverless Application Model (SAM)

* **Serverless Application Model (SAM):** AWS SAM is a framework for developing and deploying serverless applications. It simplifies the process of building applications running on AWS Lambda, Amazon API Gateway, and Amazon DynamoDB.
* **Configuration:** All the configuration in AWS SAM is done using YAML code. This provides a simple way to describe resources in your serverless application.
* **CloudFormation Integration:** AWS SAM allows you to generate complex CloudFormation templates from simple SAM YAML files. It supports all features from CloudFormation including Outputs, Mappings, Parameters, and Resources.
* **Deployment:** Deploying to AWS is simplified with AWS SAM, requiring only two commands. It can also use AWS CodeDeploy to deploy Lambda functions, providing advanced deployment capabilities.
* **Local Testing:** AWS SAM includes a local test tool to emulate your AWS Lambda and API Gateway environment locally. This allows you to test your serverless application locally before deploying.

<figure><img src="../../.gitbook/assets/image (331).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### Summary

* **Foundation:** AWS SAM is built on top of CloudFormation. It simplifies the process of creating and managing serverless applications.
* **Template Structure:** A SAM template requires two main components:
  1. **Transform Header or YAML Indication:** This indicates that it is a SAM template.
  2. **Resources Section:** This is necessary in any CloudFormation template and defines the AWS resources that make up your serverless application.
* **Key Commands:** There are several key commands you need to know:
  * `sam build`: This command builds your application, fetches dependencies, and creates local deployment artifacts.
  * `sam package`: This command packages your application, uploads it to Amazon S3, and generates a CloudFormation template.
  * `sam deploy`: This command deploys your application using CloudFormation.
* **Policy Templates:** SAM policy templates can be used for easy IAM policy definitions for your SAM functions.
* **Integration with CodeDeploy:** SAM is fully integrated with CodeDeploy to deploy your Lambda function versions to Lambda aliases.
