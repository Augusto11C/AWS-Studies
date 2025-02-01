
# Identity and Access Management - IAM

AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. Use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources.

**Users**
Instead of sharing the **root user credentials** with others, individual IAM users can be created within the aws account. Each user can have its own password for access to the AWS Management Console.

An IAM user doesn't have to represent an actual person; an IAM user can be created in order to generate an access key for an application that runs in the corporate network and needs AWS access.

**Groups**
An IAM group is a collection of IAM users. Groups allows permissions to be assigned to multiples users

**Roles**
An IAM role is an IAM identity that can be created in an aws account that **has specific permissions**. An IAM role is similar to an IAM user, in that it is an AWS identity with **permission policies** that determine what the identity can and cannot do in AWS. However, instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it. 

Roles are delegated to users , applications, or services that don't normally have access to your AWS resources. 

**Policies**
Permissions within an account can be done by using **policies**.

## AWS Policy Generator
[AWS Policy Generator](https://awspolicygen.s3.amazonaws.com/policygen.html)
