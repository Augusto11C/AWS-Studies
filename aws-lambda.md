

# Lambda

## What is Lambda?
AWS Lambda is a serverless compute service. It allows you to run codes without the help of managing servers or provisioning. You have to pay for the computing time when you consume data. There are no charges to be paid when you are not running your code. Using Lambda, you can quickly run codes for any application or backend service virtually, without any administration.

## Gathering Dependencies
![gathering-information-lambda](resources/gathering-information-lambda.png)

- Other Files: Bundle with de code all files that will be used by the function such as html for email templates.
- AWS Services: determine if the current role assigned for the lambda provides all the permissions for integration with the services that it depends on.


## Use Lambda Proxy Integration
This Config makes the integration to Lambda seamless as it enables requests to be proxied to Lambda with request details available in the event of our handler function. 
 
## How To Validate HTTP Requests Before They Reach Lambda
[ITNext - Tutorial - Validating HTTP Requst](https://itnext.io/how-to-validate-http-requests-before-they-reach-lambda-2fff68bfe93b)

## Lambda authorizer using the API Gateway
A Lambda Authorizer (formerly known as a custom authorizer) placed on an API Gateway is a Lambda function that controls access to your API endpoints. By returning a _PolicyDocument_ the lambda can decide whether or not the request is allowed to pass through to the API Gateway.

* [Enriching Requests With AWS Lambda Authorizer](https://www.theguild.nl/enriching-requests-with-an-aws-lambda-authorizer/)(Java)
* [Configure a Lambda authorizer using the API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/configure-api-gateway-lambda-authorization-with-console.html)

## [AWSLabs - aws-serverless-java-container](https://github.com/awslabs/aws-serverless-java-container)

## AWS Lambda + Spring Boot
[AWS Lambda and Java Spring Boot: Getting Started](https://epsagon.com/blog/aws-lambda-and-java-spring-boot-getting-started/)

## AWS Lambda + Micronaut
[Micronaut Functions deployed in AWS Lambda](https://guides.micronaut.io/micronaut-function-aws-lambda/guide/index.html)]
 

## References
[https://www.onlineinterviewquestions.com/aws-lambda-interview-questions/#question1](https://www.onlineinterviewquestions.com/aws-lambda-interview-questions/#question1)

[https://thenewstack.io/3-questions-serverless-technology/](https://thenewstack.io/3-questions-serverless-technology/)

[https://mindmajix.com/aws-lambda-interview-questions](https://mindmajix.com/aws-lambda-interview-questions)
