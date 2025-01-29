# Domain 2 - Security and Compliance

### AWS Shared Responsibility Model

* “Security of the Cloud”: **Responsible for protecting the infrastructure** that runs all of the services offered in the AWS Cloud. This infrastructure is composed of the hardware, software, networking, and facilities that run AWS Cloud services.

### Customer Responsibility

* "Security in the Cloud”: Responsibility will be determined by the AWS Cloud services that a customer selects. This determines the amount of configuration work the customer must perform as part of their security responsibilities.

#### Specific Controls

Controls which are solely the responsibility of the customer based on the application they are deploying within AWS services.

* Region Choices
* Service/feature Choices

### Shared Responsibility Big Picture

![](../../.gitbook/assets/2022-08-04-09-30-16.png)

```
Which of the following responsibilities would the customer manage directly, according to the AWS shared responsibility model? (pick two)

A. Applying security patches to the hypervisor for virtual machines
B. Enforcing DDoS protection for service API endpoints
C. User account management on virtual machine guest operating systems
D. Selecting the encryption key to use for protecting data at-rest
E. In-transit encryption of cross-region network traffic
```

```
Answer: C and D

All guest OS operations are the responsibility of the customer, as is the choice of encryption keys for any at- rest encryption
```

### Service Compliance Considerations

* Service availability doesn't imply all features are available in the region
* Check for service compliance by program (PCI, SOC, GDPR, etc.)
* Service compliance doesn't imply all features are compliant

### At-rest Encryption

Encryption at rest is when data at rest is given layers of encryption for security.

### In-Transit Encryption On AWS

![](../../.gitbook/assets/2022-08-10-09-31-44.png)

* The CloudFront distribution must have the DNS CNAME records listed in the configuration for TLS. ![](../../.gitbook/assets/2022-08-10-09-33-41.png)
* The API Gateway must also have the DNS CNAME records listed in the configuration for TLS.
* Classic and Network load balancers support 1 TLS cert, Application load balancers support 25.
* This cert does not require matching DNS or can even be expired as the ELB does not validate TLS.
* ACM certs must be provisioned in us-east-1 for CloudFront, otherwise in the same region as the resource.

```
When a customer chooses server side data encryption in an AWS service, who owns the Data Encryption Key (DEK)?

A. A third party, usually the owner of the root CA
B. AWS only
C. The customer only
D. AWS or the customer, depending on the service
```

```
Answer: D

When choosing server side encryption in AWS, the customer can choose .to own the master encryption key and the DEK, or can delegate .ownership of those to AWS for some services.

```

### Auditing and Reporting - CloudWatch

* Region scope
* AWS resource monitoring service
* Collect and track metrics

#### CloudWatch Logs

* Region scope
* Fault tolerant
* Durable
* Push, not pull

### Auditing and Reporting - Config

* Region scope
* Config streams
* Capture changes and configuration
* Snapshots
* Rules

### Auditing and Reporting - CloudTrail

* Region scope
* Audit trail of AWS API actions in your account
* Log successes and failures
* Multi-region trailsupport
* Organization trail support
* Transferred to S3 for long-term storage
* Searchable history
* Insights event reporting

```
Who maintains responsibility for the retention of security audit logs in AWS?

A. AWS
B. The customer
C. Both AWS and the customer
D. Neither AWS or the customer
```

```
Answer: B

The customer is 100% responsible for enabling and retaining log features in AWS.

```

### Least Privilege - RBAC and ABAC

TODO
