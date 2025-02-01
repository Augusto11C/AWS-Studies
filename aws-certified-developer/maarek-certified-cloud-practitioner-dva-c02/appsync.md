# AppSync

* **AWS AppSync**: This is a managed service that leverages GraphQL, allowing applications to efficiently fetch exactly the data they need.
* **Data Combination**: AppSync has the ability to combine data from one or more sources, which can include NoSQL data stores, relational databases, and HTTP APIs.
* **Integration**: It seamlessly integrates with various AWS services like DynamoDB, Aurora, OpenSearch, and others. It also supports custom data sources with AWS Lambda.
* **Real-time Data Retrieval**: AppSync enables real-time data retrieval using WebSocket or MQTT over WebSocket. This ensures that your application always has the most up-to-date information.
* **Mobile Apps**: For mobile applications, AppSync provides local data access and data synchronization capabilities. This allows for a smooth and responsive user experience, even in conditions of intermittent connectivity.
* **GraphQL Schema**: The journey with AppSync begins by uploading a GraphQL schema. This schema defines the structure of your data and forms the basis for operations that your application can perform.

<figure><img src="../../.gitbook/assets/image (259).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (260).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### **AppSync - Security**

* **Authorization in AWS AppSync**: There are four primary methods you can use to authorize applications to interact with your AWS AppSync GraphQL API:
  * **API\_KEY**: This method uses a simple API key for authorization. It’s straightforward and easy to implement.
  * **AWS\_IAM**: This method leverages AWS Identity and Access Management (IAM) for authorization. It allows for the use of IAM users, roles, and even cross-account access.
  * **OPENID\_CONNECT**: This method uses an OpenID Connect provider and JSON Web Tokens (JWT) for authorization. It’s a powerful and flexible option that can integrate with many different identity providers.
  * **AMAZON\_COGNITO\_USER\_POOLS**: This method uses Amazon Cognito User Pools for authorization. It’s a robust solution that integrates well with other AWS services.
* **Custom Domain & HTTPS**: If you need to use a custom domain and HTTPS with AppSync, you can place Amazon CloudFront in front of AppSync. This allows you to take advantage of CloudFront’s features, including custom domain names and HTTPS.
