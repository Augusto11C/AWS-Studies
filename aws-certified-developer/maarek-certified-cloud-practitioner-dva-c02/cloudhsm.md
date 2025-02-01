# CloudHSM

AWS CloudHSM and AWS Key Management Service (KMS) are both services offered by AWS for managing cryptographic keys, but they have some key differences:

* AWS CloudHSM:
  * CloudHSM is a cloud-based hardware security module that enables you to easily generate and use your own encryption keys on the AWS Cloud. With CloudHSM, you have exclusive, single-tenant access to each HSM in your cluster. It provides a high level of isolation for your keys and is designed to meet stringent regulatory standards.&#x20;
* AWS Key Management Service (KMS):
  * KMS is a multi-tenant, managed service that makes it easy for you to create and control the encryption keys used to encrypt your data. It is integrated with other AWS services making it easier to encrypt data you store in these services and control access to the keys that decrypt it. Key Differences:

In CloudHSM, you have full control over key management and operations, whereas in KMS, these tasks are managed by AWS. CloudHSM provides single-tenant key storage, while KMS is multi-tenant. CloudHSM is limited to a single Virtual Private Cloud (VPC), while KMS is not.
