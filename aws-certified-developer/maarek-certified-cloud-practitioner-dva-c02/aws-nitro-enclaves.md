# AWS Nitro Enclaves

**AWS Nitro Enclaves**:

* Nitro Enclaves are a feature of AWS that allows you to create highly isolated and secure compute environments for processing sensitive data. This could include Personally Identifiable Information (PII), healthcare data, financial data, credit card data, and more.
* Unlike traditional methods of creating isolated environments, such as setting up a new Virtual Private Cloud (VPC) with restricted access and networking, Nitro Enclaves provide a simpler and more secure solution.
* Nitro Enclaves are essentially virtual machines that are highly isolated and hardened. They do not have persistent storage, interactive access (you cannot SSH into them), or external networking. This makes them extremely contained and secure.
* By using Nitro Enclaves, you can reduce the attack surface for sensitive data processing applications.

**Key Features of Nitro Enclaves**:

* **Cryptographic Attestation**: This ensures that only authorized code can run in your Enclave. You sign the code, and only the signed code can run in your Enclave.
* **KMS Encryption**: With AWS Key Management Service (KMS) encryption, you can ensure that only the Enclaves can access your sensitive data.

**Use Cases**:

* Nitro Enclaves are ideal for use cases that involve processing private keys, credit cards, or secure multi-party computation.

**How it Works**:

* To create a Nitro Enclave, you launch a compatible Nitro-based EC2 instance and set the ‘EnclaveOptions’ to ‘true’. This allows you to launch a Nitro Enclave from within the EC2 instance.
* You then use the Nitro CLI to convert your application into an Enclave Image File (EIF), which you use as an input to create an Enclave on your EC2 instance.
* The Enclave shares the VP, memory, CPU, and kernel with the host but is highly isolated within it.
* The EC2 instance has separation from your Enclave and can only communicate over a secure local channel. There is also separation from any other instance running on the same host.
