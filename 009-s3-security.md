# S3 Security

# S3 Object Encryption

S3 offers several options for encrypting your data to ensure its confidentiality and integrity:

### Server-Side Encryption (SSE-S3):
- **Description:** Encrypts objects using keys managed by AWS.
- **Encryption Standard:** AES-256 encryption.
- **Default Encryption:** Enabled by default for both buckets and objects.
- **Usage:** Specify the header `x-amz-server-side-encryption: aws:s3` when uploading objects to enable SSE-S3 encryption.

### Server-Side Encryption with AWS Key Management Service (SSE-KMS):
- **Description:** Uses keys managed in AWS Key Management Service (KMS) to encrypt objects.
- **Logging:** Every use of the KMS key is logged in AWS CloudTrail.
- **Usage:** Specify the header `x-amz-server-side-encryption: aws:kms` when uploading objects. Access to the KMS service is required to read the encrypted objects.

### Server-Side Encryption with Customer-Provided Keys (SSE-C):
- **Description:** Allows you to use keys managed outside of AWS to encrypt objects.
- **Key Management:** S3 does not store the encryption key provided by the customer.
- **Usage:** Encryption key must be provided in the HTTP headers for each request. HTTPS must be used for encryption in transit.

### Client-Side Encryption:
- **Description:** Client encrypts data and keys before sending them to S3.
- **Key Management:** Customers fully manage encryption keys and data encryption process.
- **Usage:** Encrypted data is sent to S3, and clients must decrypt data themselves when retrieving it.

### Encryption In-Transit (TLS):
- **Description:** Data is encrypted during transit using HTTPS endpoints.
- **Usage:** HTTPS must be used, especially when using SSE-C to prevent data exposure over unencrypted channels.

### Default Encryption vs. Bucket Policy:
- **Default Encryption:** All objects are encrypted by default.
- **Bucket Policy:** You can enforce encryption by specifying encryption headers in the bucket policy. Bucket policies are evaluated before default encryption.

By leveraging S3's encryption options, you can ensure that your data remains secure both at rest and in transit, meeting compliance requirements and protecting sensitive information from unauthorized access.

# S3 - CORS (Cross-Origin Resource Sharing)

Cross-Origin Resource Sharing (CORS) is a mechanism that allows web applications running in one domain to request resources from another domain. Here's what you need to know about CORS in the context of Amazon S3:

### CORS Basics:
- **Same Origin:** Requests from the same origin (e.g., tarasowski.de/app1 and tarasowski.de/app2) do not require CORS headers.
- **Different Origin:** Requests from different origins (e.g., tarasowski.de and google.com) require CORS headers to be enabled on the server to allow cross-origin requests.

### Preflight Requests:
- When making a cross-origin request, the browser first sends an OPTIONS preflight request to the server to check if the cross-origin resource sharing is allowed.
- The preflight request includes CORS headers such as `Origin` to indicate the origin of the request.

### CORS Configuration in S3:
- To allow cross-origin requests to your S3 bucket, you need to enable CORS headers on the bucket.
- The CORS configuration specifies which origins are allowed to access the resources in the bucket and what HTTP methods are allowed.

### Example CORS Configuration:
```xml
<CORSConfiguration>
 <CORSRule>
   <AllowedOrigin>*</AllowedOrigin>
   <AllowedMethod>GET</AllowedMethod>
   <MaxAgeSeconds>3000</MaxAgeSeconds>
   <AllowedHeader>Authorization</AllowedHeader>
 </CORSRule>
</CORSConfiguration>
```

### Activating CORS in S3:
- CORS settings can be configured directly from the S3 Management Console or programmatically using AWS SDKs or CLI.
- Once configured, S3 will include the appropriate CORS headers in responses to cross-origin requests, allowing the browser to proceed with the request if the CORS policy permits it.

By correctly configuring CORS headers in your S3 bucket, you can ensure that cross-origin requests are handled securely and effectively, enabling seamless integration with web applications across different domains.

# S3 MFA Delete

S3 Multi-Factor Authentication (MFA) Delete adds an extra layer of security by requiring users to generate a code using their MFA device before performing critical operations on S3, such as permanently deleting objects or suspending versioning on the bucket. Here's what you need to know:

- **Purpose:** Helps prevent accidental or unauthorized deletions of objects or changes to bucket settings by requiring additional authentication.
- **Operations Requiring MFA Delete:**
  - Permanent deletion of objects.
  - Suspending versioning on the bucket.
- **Prerequisites:** MFA Delete requires versioning to be enabled on the bucket.
- **Owner Access:** Only the bucket owner (root account) can enable or disable MFA Delete for the bucket.

# S3 Access Logs

S3 Access Logs enable you to monitor and track all requests made to your S3 bucket by storing detailed log files in another designated bucket. Here's what you need to know:

- **Logging:** Each request made to the bucket is logged as a file in the designated logging bucket.
- **Same Region Requirement:** The logging bucket must be in the same AWS region as the source bucket to avoid issues and ensure efficient logging.
- **Avoid Logging Loop:** It's crucial to avoid configuring the same bucket as the logging destination to prevent a logging loop.
- **Log Analysis:** Log files are stored in a structured format and can be easily analyzed using services like Amazon Athena to gain insights into bucket usage, access patterns, and potential security issues.

# S3 Pre-signed URLs

S3 Pre-signed URLs provide temporary access to specific objects in your S3 bucket, allowing users to perform specific actions without requiring permanent credentials or direct access to the bucket. Here's what you need to know:

- **Generation Methods:** Pre-signed URLs can be generated using the S3 console, CLI, or SDKs.
- **Expiration:** URLs have a limited lifespan, ranging from minutes to hours, depending on how they are generated.
- **Use Cases:** Pre-signed URLs are useful for scenarios such as:
  - Allowing logged-in users to download premium content.
  - Temporarily granting a user the ability to upload a file to a specific location in the bucket.
- **Shareability:** Pre-signed URLs can be easily shared from the S3 console or generated programmatically using CLI commands or SDK functions.

By leveraging MFA Delete, Access Logs, and Pre-signed URLs, you can enhance the security, monitoring, and access control capabilities of your S3 buckets, ensuring that your data remains protected and accessible as needed.

# S3 Glacier Vault Lock

S3 Glacier Vault Lock enables the adoption of a WORM (Write Once Read Many) model for Glacier vaults, providing immutable storage for your data. Here's what you need to know:

- **WORM Model:** The WORM model ensures that once data is written to the vault, it cannot be modified or deleted.
- **Vault Lock Policy:** Create a vault lock policy to define the lock configuration for the vault, including retention settings.
- **Immutable Policy:** Once the vault lock policy is applied, it cannot be modified or deleted, ensuring that the data remains immutable for the specified retention period.
- **Bucket-Level Lock:** Vault lock policies are applied at the bucket level, providing granular control over data retention and immutability.

# S3 Object Lock

S3 Object Lock provides a mechanism to enforce retention periods and legal holds on S3 objects, ensuring data integrity and compliance. Here's what you need to know:

- **Versioning Requirement:** Object Lock requires versioning to be enabled on the bucket.
- **Retention Policies:**
  - **Compliance Mode:** Prevents object deletion for a specified retention period. Once set, retention cannot be shortened.
  - **Governance Mode:** Allows specified users to change retention settings and delete objects before the retention period expires.
- **Retention Period:** Specify a retention period for objects, ensuring they cannot be deleted until the period elapses.
- **Legal Hold:** Protect objects indefinitely by applying a legal hold, preventing them from being deleted until the hold is removed.
- **IAM Permissions:** Users with appropriate IAM permissions can manage legal holds and retention settings, ensuring proper governance and compliance.

By leveraging S3 Glacier Vault Lock and S3 Object Lock, you can enforce data immutability, retention policies, and legal holds, ensuring that your data remains secure, compliant, and tamper-proof.

# S3 Access Points

S3 Access Points provide a way to enforce granular access controls and permissions for specific prefixes within your S3 buckets. Here's how you can utilize them:

- **Access Point Creation:** Create separate access points for different departments or use cases, such as a "finance" access point for the `/finance` prefix and a "sales" access point for the `/sales` prefix.
- **Permission Configuration:** Configure permissions for each access point to control read and write access to the designated prefixes.
- **IAM Integration:** Users with IAM permissions can connect to specific access points based on their access requirements, ensuring least privilege access.
- **DNS Name:** Each access point is assigned its own DNS name, making it easy to identify and connect to.

# S3 Access Point VPC Origin

S3 Access Point VPC Origin allows you to restrict access to an access point so that it can only be accessed from within a specified VPC. Here's how you can set it up:

- **VPC Configuration:** Define the VPC from which the access point should be accessible.
- **VPC Endpoint Creation:** Create a VPC endpoint (gateway) to access the access point from within the VPC.
- **Endpoint Policy:** Ensure that the VPC endpoint policy allows access to both the target bucket and the specific access point, allowing traffic to flow between the VPC and the access point securely.

By leveraging S3 Access Points and VPC Origin configurations, you can enforce fine-grained access controls and restrict access to your S3 data based on specific criteria, enhancing security and compliance within your AWS environment.

# S3 Object Lambda

S3 Object Lambda introduces a powerful capability where you can dynamically process and manipulate data stored in S3 buckets on the fly. Here's how it works:

- **Bucket Setup:** You have an S3 bucket configured with access points.
- **Access Points:** Each access point is associated with a Lambda function.
- **Data Processing:** When a client retrieves data from the bucket via an access point, the request is intercepted by the associated Lambda function.
- **Data Manipulation:** The Lambda function performs dynamic data manipulation or processing before returning the modified data to the client.
- **Use Cases:**
  - Redacting Personally Identifiable Information (PII) from files.
  - Converting data formats (e.g., XML to JSON) in real-time.
  - Resizing and watermarking images on the fly.
- **Flexibility:** Object Lambda provides flexibility in data processing, enabling you to tailor the manipulation logic according to your specific requirements.
  
By leveraging S3 Object Lambda, you can implement dynamic data transformations and processing directly within your S3 infrastructure, offering enhanced flexibility and efficiency for various use cases, including data security, format conversion, and image processing.
