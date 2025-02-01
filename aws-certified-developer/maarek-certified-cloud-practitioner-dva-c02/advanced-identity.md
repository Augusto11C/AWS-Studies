# Advanced Identity

### **STS**

* **AWS Security Token Service (STS)**: This service allows you to grant limited and temporary access to AWS resources for up to 1 hour. It’s a flexible and secure way to manage access to your AWS environment.
* :warning::warning::warning:**AssumeRole**: This operation allows you to assume roles within your account or across different accounts. It’s a powerful way to delegate permissions and manage access.
* **AssumeRoleWithSAML**: This operation returns credentials for users who are authenticated via Security Assertion Markup Language (SAML).
* **AssumeRoleWithWebIdentity**: This operation returns credentials for users authenticated with a web identity provider (IdP), such as Facebook Login, Google Login, or any OpenID Connect (OIDC) compatible provider. However, AWS recommends using Cognito Identity Pools instead for enhanced security and integration.
* :warning::warning::warning:**GetSessionToken**: This operation is used for Multi-Factor Authentication (MFA), allowing you to obtain session tokens from a user or AWS account root user.
* **GetFederationToken**: This operation allows you to obtain temporary credentials for a federated user. It’s useful for managing access for users who are not in your AWS account.
* :warning::warning::warning:**GetCallerIdentity**: This operation returns details about the IAM user or role that is used in the API call. It’s a helpful way to audit and track API usage.
* **DecodeAuthorizationMessage**: This operation decodes the error message when an AWS API call is denied. It’s useful for troubleshooting and understanding permission issues.

<figure><img src="../../.gitbook/assets/image (264).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (265).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

#### STS - Assume a Role

* **IAM Role Definition**: An IAM Role should be defined within your AWS account or across multiple accounts. This role represents the permissions that you want to grant to a certain set of operations or services.
* **Principal Access**: You need to specify which principals (users, applications, or services) are allowed to assume this IAM Role. This ensures that only authorized entities can take on the role and its permissions.
* **Using AWS STS**: The AWS Security Token Service (STS) should be used to retrieve temporary security credentials. These credentials allow you to impersonate the IAM Role that you have access to, using the `AssumeRole` API operation.
* **Temporary Credentials**: The credentials provided by STS are temporary and can be valid for a duration ranging from 15 minutes up to 1 hour. This reduces the risk associated with long-term credentials, enhancing the security of your AWS environment.

<figure><img src="../../.gitbook/assets/image (263).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### STS - with MFA

* **Using GetSessionToken**: The `GetSessionToken` operation from AWS Security Token Service (STS) is used when you want to include multi-factor authentication (MFA) in your AWS operations.
* **IAM Policy**: An appropriate IAM policy should be set up using IAM conditions to enforce MFA. This policy controls who can perform actions on your AWS resources.
* **MFA Condition**: The condition `aws:MultiFactorAuthPresent:true` is used in the IAM policy to ensure that MFA is enforced. This condition checks if MFA was used during the authentication process.
* **GetSessionToken Returns**: A reminder that when you use `GetSessionToken`, it returns the following:
  * `Access ID`: This is the access key ID that identifies the temporary security credentials.
  * `Secret Key`: This is the secret access key that can be used to sign requests.
  * `Session Token`: This token is used together with the access ID and secret key to authenticate the user.
  * `Expiration Date`: This indicates when the temporary credentials expire. After this time, you must request new temporary credentials.

By using these elements, you can enhance the security of your AWS operations with MFA. It’s a powerful way to protect your resources and data.

<figure><img src="../../.gitbook/assets/image (262).png" alt=""><figcaption><p>Font: DAVIS, 2022</p></figcaption></figure>

### The `iam:PassRole`

> `PassRole` is a permission granted to IAM Users and resources that permits them to use an IAM Role.
>
> For example, imagine that there is an IAM Role called _Administrators_. This role has powerful permissions that should _not_ be given to most users.
>
> Next, imagine an IAM User who has permissions to **launch an Amazon EC2 instance**. While launching the instance, the user can specify an IAM Role to associate with the instance. If the user — who is _not_ an Administrator — were to launch an EC2 instance with the _Administrators_ role, then they could login to the instance and issue commands using permissions from that role. It would be a way for them to **circumvent permissions**: while not being an administrator themselves, they could assign the IAM Role to a resource, and then use that resource to gain privileged access.
>
> To prevent this scenario, IAM requires that the user be granted the `iam:PassRole` permission for the _Administrators_ role. If the user does not have that permission, then they will not be permitted to launch the EC2 instance as described, or to assign that role to any other services. **It gives them permission to \_pass a role**\_\*\* to a service or resource.\*\*
>
> Font: [Stackoverflow](https://stackoverflow.com/questions/63148108/understanding-iam-passrole)

> Question: Does it mean, when a non-admin user tries to launch an instance that requires admin role, it will throw error to not allow non admin user to launch instance?
>
> Answer: If a user tries to launch an Amazon EC2 instance with an IAM Role and they do not have `iam:PassRole` permissions to pass that role then, yes, the launch of the instance will be denied.

<details>

<summary>Explanation about the Stackoverflow Discution</summary>

In plain terms, the text explains the concept of "PassRole" permission in AWS Identity and Access Management (IAM). This permission allows IAM users and resources to use an IAM role. Here's a simplified explanation with examples:

Imagine there is an IAM Role called "Administrators" that has powerful permissions, intended for use by only a few trusted individuals. Now, let's say there is an IAM User who has permissions to launch an Amazon EC2 instance (a virtual server). During the instance launch process, the user can specify an IAM Role to associate with that instance.

Here's where the "PassRole" permission comes into play: If this IAM User, who is not an Administrator, attempts to launch an EC2 instance with the "Administrators" role, they could potentially gain access to powerful capabilities through that role. This would allow them to perform actions and commands that are typically reserved for administrators, even though they are not authorized as administrators themselves.

**To prevent this kind of scenario, IAM requires that the user be explicitly granted the "iam:PassRole" permission for the "Administrators" role. If the user does not have this permission, they will be denied the ability to launch the EC2 instance using the "Administrators" role. This restriction ensures that non-administrator users cannot assign roles with higher privileges to resources or services, thereby preventing them from gaining unauthorized access.**

In summary, the "PassRole" permission is a safeguard that ensures only authorized users can assign IAM roles with specific privileges to resources. If a user without the "iam:PassRole" permission tries to launch an EC2 instance with an IAM Role that they are not allowed to pass, the launch request will be denied, effectively preventing them from circumventing their own permissions and gaining unauthorized access.

</details>

### IAM - Advanced Topics

#### IAM Best Practices - General

* TODO

#### IAM Best Practices - IAM Roles

* TODO

#### IAM Best Practices - Cross Account Access

* TODO

#### Authorization Model Evaluation of Policies

* TODO

#### Dynamic Policies with IAM

* TODO

#### Inline vs Managed Policies

* TODO

### Directory Services

* AWS Managed Microsoft AD
* AD Connector
* Simple AD

<figure><img src="../../.gitbook/assets/image (266).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

### References

DAVIS, Neal. _How to Use MFA with the AWS CLI_. \[Videoaula]. YouTube, 2022. Disponível em: [https://www.youtube.com/watch?v=BNpbGHhk5Tc](https://www.youtube.com/watch?v=BNpbGHhk5Tc). Acesso em: 11 out. 2023.
