# API Gateway

### When should I use the API Gateway integration type "Lambda Function Integration" and when should I use the API Gateway integration type "Lambda Function Integration as Lambda Proxy Integration"?

In Amazon API Gateway, you can use two types of integrations with Lambda functions: "Lambda Function Integration" and "Lambda Proxy Integration." Here's an explanation of when to use each type:

1. Lambda Function Integration:
   * Use this integration when you want to define the request and response models explicitly using API Gateway's request and response models.
   * It allows you to map specific request parameters to the input parameters of the Lambda function.
   * You can configure request and response mappings to modify the payload before sending it to or after receiving it from the Lambda function.
   * This integration type provides more control over the API Gateway request and response structure.
2. Lambda Proxy Integration:
   * Use this integration when you want to pass the entire request received by API Gateway to the Lambda function without modifying the structure.
   * It allows you to use the event object received by the Lambda function to access the request data, headers, query parameters, and path parameters.
   * The Lambda function directly returns the response, which is sent back to the client without any additional modification by API Gateway.
   * This integration type is simpler and often used when you want to quickly proxy requests to Lambda without extensive request/response transformations.

In summary, use "Lambda Function Integration" when you need more control over the request and response structure, and you want to define explicit mappings. Use "Lambda Proxy Integration" when you want a simpler approach with minimal request/response transformations and want to pass the entire request to the Lambda function.

### TODO Hands-on

* Create a lambda authorizer to control access to API Gateway's endpoints.

<figure><img src="../../.gitbook/assets/image (333).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (332).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>
