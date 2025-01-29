# Key Management Service

* **AWS Key Management Service (KMS)**: Whenever you encounter the term “encryption” in the context of an AWS service, it’s highly likely that AWS KMS is involved. It’s a crucial component in securing your data on AWS.
* **Encryption Key Management**: AWS KMS manages the cryptographic keys for you. These are essential for encrypting and decrypting your data securely.
* **Integration with IAM**: AWS KMS is fully integrated with AWS Identity and Access Management (IAM), providing a robust authorization mechanism. This allows you to control who can use your keys to access your data.
* **Auditing**: You can audit the usage of your KMS keys using AWS CloudTrail. This provides a record of all key usage, helping you monitor and troubleshoot security incidents.
* **Integration with AWS Services**: AWS KMS is seamlessly integrated into most AWS services, including Amazon Elastic Block Store (EBS), Amazon Simple Storage Service (S3), Amazon Relational Database Service (RDS), and AWS Systems Manager (SSM). This makes it easy to encrypt data stored in these services.
* **Secure Coding Practices**: It’s crucial to never store your secrets in plaintext, especially within your code. Always use encryption for sensitive data.
* **API Access**: You can also access KMS key encryption through API calls using the AWS Software Development Kit (SDK) or Command Line Interface (CLI). This provides flexibility in how you encrypt and decrypt data.
* **Storing Encrypted Secrets**: Once encrypted, secrets can be safely stored in your code or environment variables. This helps protect sensitive information, even if unauthorized individuals gain access to your source code or running environment.

### Envelop Encryption

**Envelope Encryption**

* The `KMS Encrypt` API call in AWS Key Management Service (KMS) has a data limit of 4 KB. This means it can only encrypt data that is 4 KB or less in size.
* If you need to encrypt data that is larger than 4 KB, you must use a technique called Envelope Encryption. This method involves encrypting the data with a data key, and then encrypting that data key with a master key.
* The primary API operation that facilitates Envelope Encryption is the `GenerateDataKey` API. This operation generates a new data key that you can use to encrypt your data.
* :warning::warning::warning:**For exam purposes:** it’s important to remember that any data over 4 KB that needs to be encrypted must use Envelope Encryption, which in turn means using the `GenerateDataKey` API operation. This is a key aspect of managing large-scale encryption with AWS KMS.
* **GenerateDataKey API**: This API operation generates a new data key that you can use to encrypt your data. The operation returns both the plaintext key (which you can use to encrypt your data) and the encrypted version of the key (which you can store alongside the encrypted data).
* **GenerateDataKeyWithoutPlaintext API**: This API operation is similar to `GenerateDataKey`, but it does not return the plaintext version of the key. This means you cannot use it directly to perform envelope encryption, because you need the plaintext key to encrypt your data.
* **Decrypt API**: This API operation is used to decrypt both the data key and the encrypted data. You would first use it to decrypt the encrypted data key (getting back the plaintext key), and then use that plaintext key to decrypt your data.

<details>

<summary>More Discussion about Envelop Encryption</summary>

In plain terms, the statement is referring to AWS Key Management Service (KMS) and the concept of envelope encryption. Here's a simplified explanation:

Envelope encryption is a method of encrypting data where a data encryption key (DEK) is used to encrypt the actual data, and then the DEK itself is encrypted using a separate key called the key encryption key (KEK). This provides an additional layer of security for the DEK.

To perform envelope encryption using AWS KMS, there are specific API calls that need to be used. The statement mentions two specific API calls:

1. GenerateDataKey API: This API call is used to generate a data encryption key (DEK) that will be used to encrypt the data. It returns both the plaintext version of the DEK and an encrypted version of the DEK (encrypted using a key encryption key). This API call is used when you want to generate a DEK and have access to both the plaintext and encrypted versions.
2. GenerateDataKeyWithoutPlaintext API: This API call is similar to the GenerateDataKey API, but it only returns the encrypted version of the DEK. It does not provide the plaintext version of the DEK. This API call is used when you want to generate a DEK but do not need access to the plaintext version.

When it comes to decrypting data that has been encrypted with envelope encryption, the statement mentions using the "decrypt" API. The decrypt API call is used to decrypt the encrypted DEK using the appropriate key encryption key, allowing access to the plaintext DEK. Once the DEK is decrypted, it can be used to decrypt the actual data.

In order to perform envelope encryption with AWS KMS, you need to use the "GenerateDataKey" API to generate a data encryption key (DEK), and if you need access to the plaintext DEK, use the "GenerateDataKey" API. To decrypt the data, use the "decrypt" API to decrypt the encrypted DEK, which will give you access to the plaintext DEK for decrypting the actual data.

</details>

### KMS Request Quotas

* **AWS Key Management Service (KMS) Quotas**: AWS KMS has a unique feature where each cryptographic operation, including encryption and decryption, shares a quota. This quota is the limit on the number of cryptographic operations that can be performed.
* **Shared Quota**: Any service that makes a request on your behalf, such as AWS S3 using SSE-KMS data encryption, contributes to this quota. This means the quota is shared across your account for each region and across all cryptographic operations.
* **ThrottlingException**: If a key is used excessively, you may encounter a `ThrottlingException`. This is an indication that the quota for cryptographic operations has been exceeded.
* **Solutions**:
  * **Data Encryption Key (DEK) Caching**: If you’re using the `GenerateDataKey` API, you can cache the data encryption key locally. This reduces the number of API calls made to AWS, thereby reducing the usage of your quota. This is a feature of the encryption SDK itself.
  * **Request Quota Increase**: If you’re consistently exceeding your limit, you can request a quota increase. This can be done through an API call or by opening a support ticket with AWS. This will allow for more cryptographic operations to be performed.

<figure><img src="../../.gitbook/assets/image (267).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>
