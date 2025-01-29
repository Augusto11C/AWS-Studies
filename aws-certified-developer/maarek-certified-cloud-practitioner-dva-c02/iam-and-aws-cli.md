# IAM and AWS CLI

* IAM = Identity and Access Management.
* IAM is a Global Service.
* Users: are people within your organization, and can be grouped.
* Groups: only contain users, not other groups.

<figure><img src="../../.gitbook/assets/image (143).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### IAM Permissions

* IAM Permissions define the permissions of the users.

### IAM Policies

* IAM Policies are JSON documents that define a set of permissions for making requests to AWS services, and can be used by IAM Users, User Groups, and IAM Roles.
* <mark style="color:orange;">**Inline policy**</mark>
  * An inline policy is an IAM policy that is created for a single IAM identity, such as a user/role.
  * Inline policies maintain a strict one-to-one relationship between the policy and the identity to which it is applied. This means that when you delete the user in which the inline policy is embedded, the policy will also be deleted.

<details>

<summary>The <code>put-role-policy</code> command</summary>

[The `put-role-policy` command in AWS CLI is used to add or update an inline policy document that is embedded in a specified IAM role](https://docs.aws.amazon.com/cli/latest/reference/iam/put-role-policy.html). [When you embed an inline policy in a role, the inline policy is used as part of the role’s access (permissions) policy](https://docs.aws.amazon.com/cli/latest/reference/iam/put-role-policy.html).

Here’s an example of how to use the `put-role-policy` command:

```bash
aws iam put-role-policy --role-name Test-Role --policy-name ExamplePolicy --policy-document file://AdminPolicy.json
```

[In this example, the policy is defined as a JSON document in the `AdminPolicy.json` file](https://awscli.amazonaws.com/v2/documentation/api/2.4.18/reference/iam/put-role-policy.html). [The `--role-name` parameter specifies the name of the role to associate the policy with, and the `--policy-name` parameter specifies the name of the policy document](https://docs.aws.amazon.com/cli/latest/reference/iam/put-role-policy.html).

Please note that the role’s trust policy is created at the same time as the role, using `CreateRole`. [You can update a role’s trust policy using `UpdateAssumeRolePolicy`](about:blank#). [For more information about roles, see IAM roles in the IAM User Guide](about:blank#).

</details>

#### IAM Policies Structure

<figure><img src="../../.gitbook/assets/image (144).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### IAM Roles

* Some AWS services will need to perform actions on your behalf.
* Todo so, we will assign permissions to AWS services with **IAM Roles**.

<figure><img src="../../.gitbook/assets/image (145).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Who can have a IAM Role?

<figure><img src="../../.gitbook/assets/image (146).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

* **AWS Services**
* AWS Accounts
* Web identity
* SAML 2.0 Federation
* Custom Trust Policy

### IAM Security Tools

* IAM Credentials Report (account-level)
  * Tool that generates a report that lists all account's users and the status of their various credentials.
* IAM Access Advisor (user-level)
  * Tool that **shows the service permissions granted to a user** and when those services were last accessed.

### IAM Section - Summary

* Users: mapped to a physical user, has a password for AWS Console.
* Groups: Contains users only.
* Policies: JSON documente that outlines permissions for users or groups.
* Roles: The way to assign permissions to AWS Services.
* Security: MFA + Password Policy
* AWS CLI: Manage AWS Services using command-line.
* AWS SDK: Manage AWS services using a programming language.
* Access Keys: Access AWS using the CLI or SDK.
* **Audit:** IAM Credentials Reports & IAM Access Advisor.

### Security Groups

* Security Groups are the fundamental of network security in AWS.
*   They control **how traffic is allowed into or out** of Ec2 Instances.

    <figure><img src="../../.gitbook/assets/image (148).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>
* Security groups only contain **allow** rules.
* Security groups rules **can reference** by **IP** or by **security group.**
