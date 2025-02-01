# Load Balancer and API Gateway

Question/Discussion Source: [Load Balancer and API Gateway](https://repost.aws/questions/QUqqNns-YARYCQz441UFqwOw/load-balancer-and-api-gateway).

### Proposed Architecture

#### Authorizing Requests through API Gateway

1. Incoming requests should first arrive at the API Gateway for authorization (e.g. using JWT).
2. If the request is authorized, it is then forwarded to a load balancer, which will allocate the request to an EC2 farm.

#### Scaling API Gateway with Multiple Instances

1. API Gateway has a limit of 10,000 requests per second.
2. To scale higher, multiple API Gateway instances may be required within a region.
3. :question::warning: **Question:** _In this scenario, would a load balancer be needed to balance the requests among the multiple API Gateway instances?_

#### Request Flow

1. The request flow would be: ELB/ALB -> API Gateway -> ELB/ALB -> EC2 instance.
2. This three-hop approach may not be the most efficient, and alternative architectures could be considered.



### Discussion #1

1. Placing an Application Load Balancer (ALB) in front of an API Gateway may not be feasible, as the ALB's target group options do not include API Gateway as a target.
2. The 10,000 requests per second limit of API Gateway is a soft limit that can be increased based on the application's actual needs.
3. A potential solution is to create multiple API Gateway instances with different purposes.
4. An alternative, less elegant solution is to use Amazon CloudFront in front of the API Gateways, with CloudFront randomly selecting one of the API Gateways to handle the request. This is not as effective as a load balancer, but it can distribute traffic across multiple API Gateways.
5. Another solution is to bypass the API Gateway entirely and use AWS CloudFront with Lambda@Edge to handle the JWT validation and access control, protecting the Application Load Balancer's access.

### Discussion #2&#x20;

* This case is from one of the systems I worked on.
* ELB/ALB -> EC2 (Nginx) (perform autentication) -> ELB/ALB -> API Gateway -> EC2 instance/ECS/Lambda.
