# CloudFront

### Origin Request Policy

The Origin Request Policy in Amazon CloudFront is used to control the information that’s included in an origin request. When a viewer request to CloudFront results in a cache miss (the requested object is not cached at the edge location), CloudFront sends a request to the origin to retrieve the object, this is called an origin request (AWS, 2023a).

The origin request always includes the URL path, the request body (if there is one), and certain HTTP headers (AWS, 2023a):

* The URL path (the path only, without URL query strings or the domain name)
* The request body (if there is one)
* The HTTP headers that CloudFront automatically includes in every origin request, including `Host`, `User-Agent`, and `X-Amz-Cf-Id`

:warning:Other information from the viewer request, such as URL query strings, HTTP headers, and cookies, is not included in the origin request by default.

To pass other information from the request, such as URL query string, cookies, etc., the "origin request policy" can be used to control the information that is included in an origin request.

**Example Console Config**

<figure><img src="../../.gitbook/assets/image (91).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront - Origins

* S3 Buckets
  * For distributing files and caching them at the edge.
  * Enhanced security with **CloudFront Origin Access Control (OAC)**.
  * OAC is replacing Origin Access identity (OAI).
  * CloudFront can be used as an ingress (to upload files to S3).
* Custom Origin (HTTP)
  * Application Load Balancer.
  * EC2 instance.
  * S3 website (must first enable the bucket as a static S3 website).
  * Any HTTP backend you want.

<details>

<summary>Configure OAC when creating a new CloudFront distribution (AWS, 2023c)</summary>

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)
2. Choose **Create Distribution**.
3. In the **Origin** configuration section, select an S3 origin from the **Origin domain** drop-down list.
4. You can optionally configure an **origin path** to append to the origin domain name for origin requests.
5. Enter a name to uniquely identify the current origin configuration.
6. Choose Origin access control settings (or create one).

<img src="../../.gitbook/assets/image (86).png" alt="" data-size="original">

<img src="../../.gitbook/assets/image (87).png" alt="" data-size="original">

7. Follow the detailed instructions [here](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-creating-console.html) on how to configure the rest of the settings.
8. Once the distribution is successfully created, you must update the S3 bucket policy, you can reference the policy statement provided on distribution detail page (Figure 3).

8) Note that the policy provided only includes permissions to read objects from S3. If you would like to also upload objects to S3, you must update the policy with additional permissions for “_s3:PutObject_”.

</details>

### CloudFront High Level Diagram

<figure><img src="../../.gitbook/assets/image (238).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront - S3 as an Origin

<figure><img src="../../.gitbook/assets/image (239).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront vs S3 Cross Region Replication

* CloudFront:
  * Global Edge Network.
  * Files are cached for a TTL.
  * :warning: **Great for static content that must be available everywhere.**
* S3 Cross Region Replication:
  * Must be setup for each region you want replication to happen.
    * **VS CloudFront:** Amazon CloudFront does not provide fine-grained control over which specific edge locations cache your content. **When you configure CloudFront, your content is cached across multiple edge locations globally to improve latency and availability for your users (AWS, 2023b)**.
  * Files are updated in near real-time.
  * ReadOnly.
  * :warning: **Great for dynamic contend that needs to be available at low-latency in few regions.**

### CloudFront Caching

* The cache lives at each **CloudFront Edge Location**.
* CloudFront identifies each object in the cache using the Cache Key.
* You want to maximize the Cache Hit ratio to minimize requests to the origin.
* • You can invalidate part of the cache using the CreateInvalidation API.

<figure><img src="../../.gitbook/assets/image (244).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### What is CloudFront Cache Key?

* A unique identifier for every object in the cache.
* By default, consists of `hostname + resource portion of the URL`.
* If you have an application that serves up content that varies based on user, device, language, location.
* You can add other elements (HTTP headers, cookies, query strings) to the Cache Key using CloudFront Cache Policies.

<figure><img src="../../.gitbook/assets/image (245).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### CloudFront Policies – Cache Policy

* Cache based on:
  * **HTTP Headers:** None or Whitelist.
  * **Cookies:** None, or Whitelist, or Include All-Except, or All.
  * **Query Strings:** None, or Whitelist, or Include All-Except, or All.
* Control the TTL (0 seconds to 1 year), can be set by the origin using the [Cache-Control header](https://www.cloudflare.com/en-gb/learning/cdn/glossary/what-is-cache-control/), **Expires** header...
  * Note: See [No Cache-Control Header for files from AWS CloudFront with S3 Origin](https://serverfault.com/questions/770302/no-cache-control-header-for-files-from-aws-cloudfront-with-s3-origin).
* Create your own policy or use Predefined Managed Policies.
* **All HTTP headers, cookies, and query strings that you include in the Cache Key are automatically included in origin requests.**

### **CloudFront Caching - Cache Policy HTTP Headers**

* None:
  * Don't include any headers in the Cache Key (Except Default).
  * Headers are not forwarded (except default).
  * Best Caching Performance.
* Whitelist:
  * Only specified headers included in the Cache Key.
  * Specified headers are also forwarded to Origin.

<figure><img src="../../.gitbook/assets/image (246).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront Caching – Cache Policy Query Strings

* **None**
  * Don’t include any query strings in the Cache Key.
  * Query strings are not forwarded.
* **Whitelist**
  * Only specified query strings included in the Cache Key.
  * Only specified query strings are forwarded.
* **Include All-Except**
  * Include all query strings in the Cache Key except the specified list.
  * All query strings are forwarded except the specified list.
* **All**
  * Include all query strings in the Cache Key.
  * All query strings are forwarded.
  * Worst caching performance.

<figure><img src="../../.gitbook/assets/image (241).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront Policies - Origin Request Policy :warning:

* **Specify values that you want to include in origin requests without including them in the Cache Key** (no duplicated cached content).
* You can include:
  * HTTP headers: None – Whitelist – All viewer headers options.
  * Cookies: None – Whitelist – All.
  * Query Strings: None – Whitelist – All.
* Ability to add CloudFront HTTP headers and Custom Headers to an origin request that were not included in the viewer request.
* Create your own policy or use Predefined Managed Policies.

### Cache Policy vs. Origin Request Policy

<figure><img src="../../.gitbook/assets/image (242).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront - Cache Invalidations

* In case you update the back-end origin, CloudFront doesn’t know about it and will only get the refreshed content after the TTL has expired.
* However, you can force an entire or partial cache refresh (thus bypassing the TTL) by performing a CloudFront Invalidation.
*   Example:

    * You can invalidate all files (`*`_) or a special path (`/images/*`_).

    <figure><img src="../../.gitbook/assets/image (243).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront - Cache Behaviors

* Configure different settings for a given URL path pattern.
* Example: one specific cache behavior to images/\*.jpg files on your origin web server.
* Route to different kind of origins/origin groups based on the **content type** or **path pattern**:
  * `/images/*`
  * `/api/*`
  * `/*` (default cache behavior).
* :warning:When adding additional Cache Behaviors, the Default Cache Behavior is always the last to be processed and is always `/*`.

<figure><img src="../../.gitbook/assets/image (79).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### CloudFront - Cache Behaviors - Sign In Page

<figure><img src="../../.gitbook/assets/image (80).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront - Maximize cache hits by separating static and dynamic distributions

<figure><img src="../../.gitbook/assets/image (81).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront - ALB or EC2 as an origin

<figure><img src="../../.gitbook/assets/image (84).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront - Signed URLs

* Use Case: You want to distribute paid shared content to premium users over the world.
* We can use CloudFront Signed URL / Cookie. We attach a policy with:
  * Includes URL Expiration.
  * Includes IP ranges to access the data from.
  * Trusted signers (which AWS accounts can create signed URLs).
* How long should the URL be valid for?
  * Shared content (move, music): make it short (a few minute).
  * Private content (private to the user): you can make it last for years.
* Signed URL = Access to individuals files (one URL per file).
* Signed Cookies = access to multiple files (one signed cookie for many files).

#### Proccess for Creating and Using Signed URLs

How signed URLs work with CloudFront and Amazon S3 (AWS, 2023b):

1. **Configuration**: To set up signed URLs, you configure your CloudFront distribution by specifying trusted key groups that contain public keys. These keys are used to verify the signature of the URLs. You use corresponding private keys to sign the URLs.
2. **Developing the Application**: Your application is responsible for determining whether a user should have access to certain content and for creating signed URLs for restricted content. There are two methods for creating signed URLs: using a canned policy or a custom policy.
3. **User Request**: When a user wants to access a file that requires a signed URL, your application verifies the user's entitlement to access it, such as by checking if they've signed in, paid for access, or met other requirements.
4. **Generating Signed URLs**: After verification, your application generates and provides the user with a signed URL, allowing them to download or stream the content. This step is usually seamless for the user, especially when accessing content through a web browser.
5. **Signature Validation**: CloudFront uses the public key to validate the URL's signature, ensuring it hasn't been tampered with. If the signature is invalid, the request is rejected.
6. **Policy Confirmation**: If the signature is valid, CloudFront checks the policy statement in the URL (or constructs one for canned policies) to confirm if the request aligns with the specified access parameters, such as time restrictions.
7. **Content Delivery**: If the request meets the policy requirements, CloudFront proceeds with standard operations. It checks if the file is in the edge cache, forwards the request to the origin if needed, and delivers the file to the user.

#### CloudFront Signed URL Diagram

<figure><img src="../../.gitbook/assets/image (85).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront Signed URL vs S3 Pre-Signed URL

### CloudFront Geo Restriction

* You can restrict who can access your distribution.
  * Allowlist and Blocklist.
* The "country" in determined using a 3 party Geo-IP database.
* Use Case: Copyright Laws to control access to contend.

<figure><img src="../../.gitbook/assets/image (240).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront - Price Classes

* You can reduce the number of edge locations for **cost reduction.**
* Three price classes:
  * Price Class All: all regions – best performance.
  * Price Class 200: most regions, but excludes the most expensive regions.
  * Price Class 100: only the least expensive regions.

<figure><img src="../../.gitbook/assets/image (88).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (89).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront - Multiple Origin

* To route to different kind of origins based on the content type.
* Based on path pattern:
  * `/images/*`
  * `/api/*`
  * `/*`

<figure><img src="../../.gitbook/assets/image (74).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront - Origin Groups

* To increase high-availability and do failover.
* Origin Group: one primary and one secondary origin.
* If the primary origin fails, the second one is used.

<figure><img src="../../.gitbook/assets/image (75).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (76).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

CloudFrount - Field Level Encryption

* Protect user sensitive information through application stack.
* Adds an additional layer of security along with HTTPS.
* Sensitive information encrypted at the edge close to user.
* Uses asymmetric encryption.
* Usage:
  * Specify set of fields in POST requests that you want to be encrypted (up to 10 fields).
  * Specify the public key to encrypt them.

<figure><img src="../../.gitbook/assets/image (77).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### CloudFront - Real Time Logs

* Get real-time requests received by CloudFront sent to Kinesis Data Streams.
* Monitor, analyze, and take actions based on content delivery performance.
* Allows you to choose:
  * Sampling Rate – percentage of requests for which you want to receive.
  * Specific fields and specific Cache Behaviors (path patterns).

<figure><img src="../../.gitbook/assets/image (78).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### References

AWS. _Controlling origin requests_. Amazon Web Services. Disponível em: <[https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/controlling-origin-requests.html](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/controlling-origin-requests.html)>. Acesso em: 22 set. 2023a.

AWS. _How caching works with CloudFront edge locations_. Amazon Web Services. Disponível em: <[https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cache-hit-ratio-explained.html](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cache-hit-ratio-explained.html)>. Acesso em: 24 set. 2023b.

AWS. _Creating a distribution_. Amazon Web Services. Disponível em: <[https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-creating-console.html](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-creating-console.html)>. Acesso em: 24 set. 2023c.
