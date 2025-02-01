# S3 - Advanced

### Amazon S3 - Moving between Storage Classes

* You can transition objects between storage classes.
* For infrequently accessed object, move them to **Standard IA.**
* For achive objects that you don't need fast access to, move them to **Glacier** or **Glacier Deep Archive.**
* Moving Objects can be automated using a **Lifecycle Rules**.

<figure><img src="../../.gitbook/assets/image (222).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### Amazon S3 - Lifecycle Rules

<figure><img src="../../.gitbook/assets/image (224).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

* **Transition Actions:** Configure objects to transition to another storage class.
  * Move objects to Standard IA Class 60 days after creation.
  * Move to Glacier for archiving after 6 months.
* **Expiration Actions:** Configure objects to expire (delete) after some time.
  * Access log files can be set to delete after a 365 days.
  * Can be used to **delete old versions** of files (**if versioning is enabled**).
  * Can be used to delete incomplete Multi-Part Uploads.
* Rules can be created for a certain prefix (example: `s3://mybucket/mp3/*`).
* Rules can be created for certain objects Tags ( example: `Department: Finance`).

**Lifecycle Rules Actions:**

* Move current versions of objects between storage classes.
* Move noncurrent versions of objects between storage classes.
* Expire current versions of objects.
* Permanently delete noncurrent versions of objects.
* Delete expired object delete markers or incomplete multipart uploads.

<figure><img src="../../.gitbook/assets/image (223).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

#### Amazon S3 - Lifecycle Rules (Scenario 1)

* Your application on EC2 creates images thumbnails after profile photos are uploaded to Amazon S3.
* These thumbnails can be easily recreated, and only need to be kept for 60 days.
* The source images should be able to be immediately retrieved for these 60 days, and afterwards, the user can wait up to 6 hours.
* **How would you design this?**
  * S3 source images can be on **Standard**, with a **lifecycle configuration** to transition them to **Glacier** after 60 days.
  * S3 thumbnails can be on **One-Zone IA**, with a **lifecycle configuration** to **expire** them (delete them) after 60 days.

#### Amazon S3 - Lifecycle Rules (Scenario 2)

* A rule in your company states that you should be able to **recover** your **deleted S3 objects immediately** for 30 days (although this may happen rarely).
* After this time, and for up to 365 days, deleted objects should be recoverable within 48 hours.
* **How would you design this?**
  * Enable S3 Versioning in order to have object versions, so that “deleted objects” are in fact hidden by a “delete marker” and can be recovered.
  * Transition the “**noncurrent versions**” of the object to **Standard IA.**
  * Transition afterwards the “**noncurrent versions**” to **Glacier Deep Archive.**

### Amazon S3 Analytics - Storage Class Analysis

* Help you decide when to transition objects to the right storage class.
* Recommendations for **Standard** and **Standard IA.**
* :warning:Does NOT work for **One-Zone IA** or **Glacier.**
* Report is updated daily.
* 24 to 48 hours to start seeing data analysis coming out of it.
* output of this service: .csv.

<figure><img src="../../.gitbook/assets/image (226).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (225).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>

### S3 Event Notifications

* Notify when: S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication…
* Object name filtering possible (\*.jpg).
* **Use case:**
  * Generate thumbnails of images uploaded to S3.
* Can create as many “S3 events” as desired.

<figure><img src="../../.gitbook/assets/image (227).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

**S3 Event Notifications - IAM Permissions**

<figure><img src="../../.gitbook/assets/image (228).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### S3 Event Notifications with Amazon EventBridge

* Advanced filtering options with JSON rules (metadata, object size, name...).
* Multiple Destinations – ex Step Functions, Kinesis Streams / Firehose…
* EventBridge Capabilities – Archive, Replay Events, Reliable delivery.

<figure><img src="../../.gitbook/assets/image (237).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### S3 - Baseline Performance

* Amazon S3 automatically scales to high request rates, latency 100-200 ms.
* Your application can achieve at least **3,500 PUT/COPY/POST/DELETE** or **5,500 GET/HEAD** requests per second per prefix in a bucket.
* There are no limits to the number of prefixes in a bucket.
* Example (object path => prefix):
  * `bucket/folder1/sub1/file` => `/folder1/sub1/`
  * `bucket/folder1/sub2/file` => `/folder1/sub2/`
  * `bucket/1/file` => `/1/`
  * `bucket/2/file` => `/2/`
* If you spread reads across all four prefixes evenly, you can achieve 22,000 requests per second for GET and HEAD.

### S3 Performance

#### Multi-Part upload

* Recommended for files > 100MB.
* Can help parallelize uploads (speed up transfers).

<figure><img src="../../.gitbook/assets/image (229).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<details>

<summary><span data-gb-custom-inline data-tag="emoji" data-code="1f44d">👍</span> How Does Multi-Part Upload Process Work?</summary>

Multipart upload is a feature in Amazon S3 that allows you to upload large objects in parts, which can improve reliability, speed, and efficiency when dealing with very large files. Here's an overview of how the multipart upload process for S3 works (AWS, 2023a):

1. **Initialization**: To initiate a multipart upload, you send a request to Amazon S3, specifying the name of the object and the associated bucket. Amazon S3 responds with a unique upload ID, which is used to identify this specific multipart upload session. This upload ID is required for all subsequent operations related to this upload.
2. **Uploading Parts**: Next, you can start uploading parts of your object. Each part is a portion of the overall object's data, and they can be of varying sizes. You specify the part number for each part you upload, and you must include the upload ID obtained in the initialization step.
3. **Part Ordering**: It's important to upload the parts in the correct order, as Amazon S3 will later concatenate these parts based on their part numbers to reconstruct the complete object. Part numbers must be integers from 1 to 10,000.
4. **Tracking ETags**: After uploading each part, Amazon S3 responds with an ETag, which is essentially a checksum of the data. You should save these ETags for each part, as they are needed later to verify the integrity of the uploaded parts during the completion step.
5. **Listing Parts**: At any point during the upload process, you can list the parts that have been successfully uploaded for a specific multipart upload session using the upload ID. This allows you to keep track of the progress.
6. **Completing the Upload**: Once all parts have been successfully uploaded, you send a "complete" request to Amazon S3, including the list of part numbers and their corresponding ETags. Amazon S3 then concatenates the parts in the specified order to create the final object. If any part is missing or has an incorrect ETag, the complete request will fail, ensuring data integrity.
7. **Abort or Cancel**: If, for any reason, you need to cancel the multipart upload before completing it, you can send an "abort" request with the upload ID. This removes any parts that have been uploaded, freeing up resources and preventing unnecessary charges.
8. **Object Availability**: Once the multipart upload is successfully completed, the object is available for retrieval through Amazon S3, just like any other object. You can read, modify, or delete it as needed.

</details>

#### S3 Transfer Acceleration

* Increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region.
* :warning: Compatible with multi-part upload.

<figure><img src="../../.gitbook/assets/image (230).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<details>

<summary>How Does S3 Transfer Acceleration Work?</summary>

Amazon S3 Transfer Acceleration works as described below (AWS, 2023b):

1.  **Accelerated Endpoints**: When you enable Transfer Acceleration for an Amazon S3 bucket, AWS creates a unique endpoint for that bucket, which has an "accelerated" URL. This URL is different from the standard S3 bucket URL.\\

    <figure><img src="../../.gitbook/assets/image (231).png" alt=""><figcaption><p>Font: AWS Console, 2023</p></figcaption></figure>
2. **Data Transfer**: When you send an HTTP request to the accelerated URL for uploading or downloading objects, the request is automatically routed to the nearest CloudFront edge location. These edge locations are strategically distributed around the world.
3. **Edge Caching**: CloudFront edge locations act as caching points. If the object you're requesting is already cached at the nearest edge location, CloudFront delivers it directly to you, significantly reducing latency. If it's not cached, CloudFront retrieves the object from the source S3 bucket.
4. **Optimized Route**: CloudFront dynamically selects the best path for data transfer based on network conditions, reducing the distance data needs to travel and minimizing packet loss and network congestion. This optimized routing helps improve transfer speed.
5. **Parallelism**: Transfer Acceleration supports concurrent multipart uploads, allowing you to upload parts of a large object in parallel. This parallelism can significantly increase upload speed, especially for large files.
6. **Security**: Data transferred via Transfer Acceleration is encrypted in transit, just like standard S3 transfers. It maintains the security features of Amazon S3, including authentication and access control through IAM (Identity and Access Management) policies and bucket policies.

</details>

### S3 Performance – S3 Byte-Range Fetches

* Parallelize GETs by requesting specific byte ranges.
* Better resilience in case of failures.
* Can be used to speed up downloads.
* Can be used to retrieve only partial data (for example the head of a file).

### S3 Select and Glacier Select

* Retrieve less data using SQL by performing server-side filtering.
* Can filter by rows & columns (simple SQL statements).
* Less network transfer, less CPU cost client-side.

[S3 Select and Glacier Select – Retrieving Subsets of Objects](https://aws.amazon.com/blogs/aws/s3-glacier-select/)

<figure><img src="../../.gitbook/assets/image (232).png" alt=""><figcaption><p>Font: HUNT, 2017</p></figcaption></figure>

**Example of S3 Select Use:**

<figure><img src="../../.gitbook/assets/image (235).png" alt=""><figcaption><p>Font: PATEL, 2021</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (234).png" alt=""><figcaption><p>Font: PATEL, 2021</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (236).png" alt=""><figcaption><p>Font: PATEL, 2021</p></figcaption></figure>

### S3 User-Defined Object Metadata & S3 Object Tags

* S3 User-Defined Object Metadata:
  * When uploading an object, you can also assign metadata.
  * Name-value (key-value) pairs.
  * User-defined metadata names must begin with "**x-amz-meta-**”.
  * Amazon S3 stores user-defined metadata keys in lowercase.
  * Metadata can be retrieved while retrieving the object.
* S3 Object Tags:
  * Key-value pairs for objects in Amazon S3.
  * Useful for fine-grained permissions (only access specific objects with specific tags).
  * :warning:Useful for **analytics** **purposes** (using S3 Analytics to group by tags).
* You **cannot search the object** metadata or object tags. Instead, you must use an external DB as a search index such as DynamoDB.

### References

AWS. _Uploading and copying objects using multipart upload_. Amazon Web Services. Disponível em: <[https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpuoverview.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpuoverview.html)>. Acesso em: 22 set. 2023a.

AWS. _Configuring fast, secure file transfers using Amazon S3 Transfer Acceleration_. Amazon Web Services. Disponível em: <[https://docs.aws.amazon.com/AmazonS3/latest/userguide/transfer-acceleration.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/transfer-acceleration.html)>. Acesso em: 22 set. 2023b.

HUNT, Randall. _S3 Select and Glacier Select – Retrieving Subsets of Objects_. AWS Blog, 2017. Disponível em: <[https://aws.amazon.com/blogs/aws/s3-glacier-select/](https://aws.amazon.com/blogs/aws/s3-glacier-select/)>. Acesso em: 22 set. 2023.

PATEL, Harshil. _Query S3 with SQL using S3 Select_. Arctype, 2021. Disponível em: <[https://arctype.com/blog/s3-select/](https://arctype.com/blog/s3-select/)>. Acesso em: 22 set. 2023.
