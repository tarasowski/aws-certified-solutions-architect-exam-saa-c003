# S3

# S3 Basics

Amazon S3 (Simple Storage Service) provides a highly scalable, durable, and secure storage solution, capable of growing to meet the needs of businesses of any size. Here are the key concepts and features:

### Key Features:
- **Infinitely Scalable Storage:** S3 can grow as needed to store an unlimited amount of data.
- **Backup and Storage:** Ideal for backing up data and storing large amounts of data.
- **Disaster Recovery:** Allows for disaster recovery across different regions to ensure data availability and durability.
- **Data Lakes:** Can be used to build data lakes for analytics and big data processing.

### Data Storage in S3:
- **Buckets:** Data in S3 is stored in containers called buckets. Each bucket name must be globally unique.
- **Objects:** Files stored in S3 are referred to as objects. Each object consists of the file data and metadata.
- **Naming Conventions:** Bucket names must not contain uppercase letters or underscores.
- **Keys:** Each object in S3 is identified by a unique key, which is essentially the full path to the object. The key is a combination of a prefix (similar to a directory path) and the object name.

### Object Details:
- **Object Content:** The actual data stored in the object is known as the object value or body.
- **Maximum Size:** A single object can be up to 5 TB in size. For objects larger than 5 GB, it is recommended to use multi-part upload.
- **Versioning:** If versioning is enabled on a bucket, each object has a version ID, allowing you to keep multiple versions of the same object.

### Access and Security:
- **Presigned URLs:** When you want to share an object with someone, you can generate a presigned URL. This URL includes temporary credentials and is only valid for a limited period, ensuring that access is controlled and secure.

### Additional Considerations:
- **No Directories:** While the key structure might suggest directories, S3 does not have a true directory hierarchy. The key's prefix can be used to simulate directories for organizational purposes.
- **Multipart Upload:** For large files exceeding 5 GB, S3 supports multipart upload, allowing you to upload parts of the file in parallel, improving efficiency and resilience.

Amazon S3 is a powerful tool for managing and storing data at scale, with features designed to ensure data durability, availability, and security.

# Amazon S3 Security

### Types of Policies:
- **User-based Policies:** Managed through AWS IAM (Identity and Access Management) policies. These policies define permissions for users or roles.
- **Resource-based Policies:** Include bucket policies and object access control lists (ACLs).
  - **Bucket Policies:** Commonly used to control access to an entire bucket. 
  - **Object ACLs:** Fine-grained control over individual objects.

### Key Concepts:
- **Principal:** The AWS account or user to which the policy is applied.
- **Cross-Account Access:** To allow another AWS account to access your bucket, you must create a bucket policy that grants the necessary permissions.
- **Public Access Settings:** Even if a bucket policy allows public access, the "Block all public access" setting in the S3 console must be disabled for the policy to take effect.
- **Policy Scope:** A policy with `arn::eu-central-1::bucket-name/*` applies to all objects within the specified bucket.

# S3 - Static Website Hosting

- To host a static website on S3, the bucket must be made public, and the "Block all public access" setting must be disabled.

# S3 - Versioning

### How Versioning Works:
- **Bucket-Level Setting:** Versioning is enabled at the bucket level.
- **Version Creation:** Uploading the same key multiple times creates new versions (e.g., version 1, 2, 3).
- **Deletion Marker:** Deleting a file adds a deletion marker rather than removing the file.
- **Easy Rollback:** Allows rolling back to previous versions of a file.
- **Version Null:** Files uploaded before versioning was enabled have a version ID of null.
- **Version-Specific Deletion:** To rollback, delete the specific version. Deleting a version removes the deletion marker, but the original object remains in the bucket.

Amazon S3 provides robust security features, flexible website hosting options, and advanced versioning capabilities to manage and protect your data effectively.

# S3 - Replication

### Types of Replication:
- **CRR (Cross-Region Replication):** Replicates objects across different AWS regions.
- **SRR (Same-Region Replication):** Replicates objects within the same AWS region.

### Key Requirements:
- **Enable Versioning:** Versioning must be enabled on both the source and target buckets.
- **Asynchronous Replication:** Replication occurs asynchronously, meaning there may be a delay before the replicated objects appear in the target bucket.
- **IAM Permissions:** Proper IAM permissions must be granted to S3 to perform replication.

### Replication Details:
- **New Objects:** Only new objects added to the source bucket are replicated automatically.
- **Existing Objects:** To replicate existing objects, use the S3 Batch Replication feature.
- **Delete Operations:** 
  - Delete markers can be replicated from the source to the target bucket if configured.
  - By default, delete markers are not replicated, but this can be activated if needed.
  - Original objects that are deleted in the source bucket are not replicated to the target bucket.

### Replication Rules:
- **No Chaining:** Replication does not chain. For example, if Bucket 1 replicates to Bucket 2, and Bucket 2 replicates to Bucket 3, changes in Bucket 1 will not automatically propagate to Bucket 3.
- **Demo Replication Rules:** Create replication rules to specify how replication should occur, including which objects to replicate and where to replicate them.

### Versioning and Replication:
- **Version IDs:** The version IDs of objects are replicated along with the objects.
- **Delete Markers:** By default, delete markers are not replicated, but this can be configured. 

### Additional Notes:
- **Replication Activation:** Replication functionality only works if versioning is enabled on the buckets involved.

Amazon S3 replication provides a robust solution for copying objects within or across regions, ensuring data availability, redundancy, and compliance with regulatory requirements.

# S3 Storage Classes

Amazon S3 offers a variety of storage classes designed to accommodate different use cases, access patterns, and cost requirements. Below are the main storage classes available in S3:

### Storage Classes:

1. **Standard General Purpose:**
   - **Description:** For frequently accessed data.
   - **Features:** Low latency, high throughput, can sustain 2 concurrent facility failures.
   - **Use Cases:** Big data analytics, mobile gaming, content distribution.
   - **Durability:** 99.999999999% (11 nines).
   - **Availability:** 99.99%.

2. **Standard Infrequent Access (IA):**
   - **Description:** For infrequently accessed data that still requires rapid access.
   - **Features:** Lower cost than Standard, but with slightly higher retrieval costs.
   - **Use Cases:** Disaster recovery, backups.
   - **Durability:** 99.999999999% (11 nines).
   - **Availability:** 99.9%.

3. **One-Zone Infrequent Access:**
   - **Description:** High durability within a single Availability Zone.
   - **Features:** Lower cost, but data is lost if the AZ is destroyed.
   - **Use Cases:** Secondary backups of on-premise data, data that can be easily recreated.
   - **Durability:** 99.999999999% (11 nines) in a single AZ.
   - **Availability:** 99.5%.

4. **Glacier Instant Retrieval:**
   - **Description:** For archived data that requires milliseconds retrieval time.
   - **Features:** Great for data accessed once a quarter, minimum storage duration of 90 days.
   - **Use Cases:** Archiving data with occasional access.
   - **Durability:** 99.999999999% (11 nines).

5. **Glacier Flexible Retrieval:**
   - **Description:** For long-term archive with flexible retrieval options.
   - **Features:**
     - Expedited retrieval: 1 to 5 minutes.
     - Standard retrieval: 3 to 5 hours.
     - Bulk retrieval: 5 to 12 hours.
     - Minimum storage duration: 90 days.
   - **Use Cases:** Archiving data with less frequent access needs.
   - **Durability:** 99.999999999% (11 nines).

6. **Glacier Deep Archive:**
   - **Description:** Lowest-cost storage class for archiving data that rarely needs to be accessed.
   - **Features:**
     - Standard retrieval: 12 hours.
     - Bulk retrieval: 48 hours.
     - Minimum storage duration: 180 days.
   - **Use Cases:** Long-term data archiving.
   - **Durability:** 99.999999999% (11 nines).

7. **Intelligent Tiering:**
   - **Description:** Automatically moves objects between different access tiers based on changing access patterns.
   - **Features:** No retrieval charges in S3 Intelligent-Tiering.
     - **Frequent Access Tier:** Default tier for frequently accessed data.
     - **Infrequent Access Tier:** For data not accessed for 30 days.
     - **Archive Instant Access Tier:** For data not accessed for 90 days.
     - **Archive Access Tier:** Optional tier for data not accessed for 90-700+ days.
     - **Deep Archive Access Tier:** Optional tier for data not accessed for 180-700+ days.
   - **Use Cases:** Data with unpredictable access patterns.

### Moving Objects Between Storage Classes:

- **Lifecycle Rules:** You can set up lifecycle rules to automatically transition objects between different storage classes based on specified conditions.
- **Manual Transfer:** Objects can be manually moved between storage classes when created or using the S3 lifecycle configuration.

### Definitions:

- **Durability:** Measures how reliably data can be stored, typically resulting in an average loss of an object once in 10,000 years.
- **Availability:** Measures how readily available a service is for use.

Amazon S3's variety of storage classes and flexible management options ensure that you can optimize your storage costs and performance according to your specific needs.
