# S3 Advanced

## S3 Lifecycle Rules

S3 Lifecycle rules allow you to automate the management of your objects to optimize storage costs. Here are the key components and functionalities:

### Transition Actions:
- **Description:** Automatically move objects to a different storage class after a specified number of days since creation.
- **Example:** Move objects to a cheaper storage class (e.g., from Standard to Standard-IA) 60 days after creation.

### Expiration Actions:
- **Description:** Automatically delete objects after they have been stored for a specified period.
- **Example:** Configure objects to be deleted 365 days after their creation date to manage storage costs and comply with data retention policies.

### Scope of Lifecycle Rules:
- **Tags:** Apply lifecycle rules to objects with specific tags.
- **Buckets:** Apply lifecycle rules to all objects within a bucket.
- **Object Names:** Apply lifecycle rules to objects with specific name patterns (prefixes).

### Versioning:
- **Requirement:** To recover deleted objects using lifecycle rules, versioning must be enabled on the bucket.
- **Benefits:** Versioning allows you to retain and restore previous versions of objects, adding an additional layer of data protection.

### S3 Analytics:
- **Description:** Provides insights and recommendations on how to optimize storage costs by analyzing access patterns and usage.
- **Functionality:** Generates reports that help identify objects that are candidates for transitioning to more cost-effective storage classes.

## Summary

Amazon S3 provides advanced features like Lifecycle rules and S3 Analytics to help manage storage costs and efficiency:

- **Lifecycle Rules:** Automate transitions and deletions based on object age, tags, buckets, or object names.
- **Versioning:** Essential for recovering objects that have been transitioned or expired.
- **S3 Analytics:** Offers reports and recommendations to optimize storage costs by suggesting transitions based on usage patterns.

These advanced features enable efficient and cost-effective storage management in S3.

# S3 Request Pays

In traditional S3 setups, the bucket owner bears all the costs associated with storage and operations within the bucket. However, with the introduction of Requester Pays, the dynamics change:

### Key Points:

- **Cost Responsibility:** With Requester Pays, the individual or entity making the data download request pays for the associated data transfer costs, such as data egress charges.
  
- **Bucket Owner Responsibilities:** The bucket owner continues to cover all storage costs and other charges associated with the bucket itself.

- **Authentication Requirement:** To initiate a Requester Pays download, the requester must be authenticated with AWS, ensuring accountability and security.

Requester Pays is particularly useful in scenarios where data access is initiated by parties external to the bucket owner, such as sharing data with external collaborators or providing access to publicly available datasets. It helps distribute the cost burden more equitably among users accessing the data.

# S3 Event Notifications

S3 Event Notifications enable you to automate workflows and trigger actions in response to specific events that occur within your S3 bucket. Here are the key components and functionalities:

### Event Types:
- **s:ObjectCreated:** Triggered when a new object is created in the bucket.
- **Filtering:** You can filter events based on criteria such as file extensions (*.jpg) to only trigger actions for specific types of objects.

### Use Cases:
- **Thumbnail Generation:** Automatically generate thumbnails of images upon upload to S3.
- **Workflow Automation:** Trigger downstream processes such as data processing, analysis, or archival based on object creation events.

### Delivery Speed:
- **Real-time Delivery:** Events are delivered within seconds of the triggering action, ensuring timely responsiveness to changes in the S3 bucket.

### Resource Access Policy:
- **SNS Resource Access Policy:** To send event notifications to other AWS services like SNS (Simple Notification Service), SQS (Simple Queue Service), or Lambda, you need to attach an appropriate resource access policy to your S3 bucket.
- **Access Policy vs. IAM Roles:** Instead of using IAM roles, S3 event notifications rely on access policies. You modify the access policy on the target (e.g., SNS, SQS, Lambda) to grant permissions for S3 to send events to those services.

### Integration Options:
- **SNS, SQS, Lambda:** Common targets for S3 event notifications. You can configure S3 to send notifications directly to these services, triggering custom actions or workflows.
- **EventBridge (formerly CloudWatch Events):** Alternatively, you can route all S3 events to EventBridge and set up rules within EventBridge to trigger actions across a wide range of AWS services, providing more centralized event management and routing capabilities.

S3 Event Notifications offer a powerful mechanism for automating processes and reacting to changes within your S3 buckets, enabling seamless integration and workflow automation across your AWS environment.

# S3 Performance

Amazon S3 offers robust performance capabilities to handle a high volume of requests efficiently. Here are some key aspects of S3's performance:

### Scalability:
- **High Request Rate:** S3 can handle a high number of requests, typically responding within 100-200 milliseconds.
- **Concurrency:** Supports a large number of concurrent requests, allowing for high throughput operations.

### Request Limits:
- **PUT Requests:** Up to 3500 PUT requests per second per prefix in a bucket.
- **GET Requests:** Up to 5500 GET requests per second per prefix in a bucket.
  - *Note:* The prefix is the part of the object key before the first slash ("/") in the object's key name. For example, in "bucket1/file", the prefix is "bucket1".

### Best Practices for Large Files:
- **Multi-part Upload:** Recommended for files larger than 100 MB and required for files larger than 5 GB.
  - *Benefits:* Improves reliability, efficiency, and speed of uploads for large files by breaking them into smaller parts.

S3's performance capabilities make it suitable for a wide range of use cases, from small object storage to handling large-scale data transfers and storage needs. By leveraging multi-part upload and understanding request limits, you can optimize the performance of your S3 operations effectively.

# S3 Transfer Acceleration

S3 Transfer Acceleration enhances data transfer speed by leveraging AWS edge locations as intermediate points. Here's how it works:

- **Increased Speed:** Files are initially transferred to the nearest AWS edge location, which then forwards the data to the designated S3 bucket in the target region.
- **Multi-part Upload Compatibility:** S3 Transfer Acceleration is compatible with multi-part uploads, allowing for large file uploads to be accelerated as well.
- **Edge Locations:** With over 200 edge locations globally, data can be quickly transferred to and from these points, significantly reducing latency and improving transfer speeds compared to traditional transfer methods.

This feature is particularly useful for scenarios where data needs to be transferred quickly across regions or when dealing with large files that can benefit from accelerated upload speeds.

# S3 Byte-Range Fetches

S3 Byte-Range Fetches enable the parallelization of GET requests by requesting specific byte ranges of a file. Here are its key features:

- **Improved Resilience:** By fetching data in parallel and requesting specific byte ranges, byte-range fetches provide better resilience in case of network failures or interruptions during the download process.
- **Speed Optimization:** Byte-range fetches can be used to speed up downloads by fetching different parts of the file in parallel, maximizing bandwidth usage and reducing overall download time.
- **Partial Data Retrieval:** Additionally, byte-range fetches allow for retrieving only specific portions of a file, such as the head or tail, rather than downloading the entire file. This can be useful for scenarios where only a subset of the data is required.

By leveraging byte-range fetches, users can optimize download performance, increase resilience, and efficiently retrieve partial data from S3 objects, enhancing overall data access capabilities within the S3 ecosystem.

# S3 Select & Glacier Select

S3 Select and Glacier Select offer powerful capabilities for querying and filtering data stored in Amazon S3 and Glacier using SQL expressions. Here's what you need to know:

- **SQL Queries:** These services allow you to perform server-side filtering using simple SQL statements, enabling you to filter data by rows and columns.
- **Reduced Network Transfer:** By performing filtering on the server-side, only the selected data is transferred over the network, reducing both network transfer costs and client-side CPU usage.
- **Efficiency:** Server-side processing reduces the need to transfer and process large volumes of data locally, leading to faster query execution times and improved efficiency.

These features are particularly useful for applications that require querying large datasets stored in S3 or Glacier, allowing for efficient and cost-effective data retrieval and analysis.

# S3 Batch Operations

S3 Batch Operations provide a way to perform bulk operations on existing S3 objects, enabling various management and optimization tasks:

- **Bulk Operations:** Perform actions such as encryption of unencrypted objects, modification of ACLs, restoration of objects from S3 Glacier, modification of object metadata, and invocation of Lambda functions for custom actions on each object.
- **Efficiency:** S3 Batch Operations allow you to process large numbers of objects in parallel, improving efficiency and reducing the time required to perform bulk tasks.
- **Integration with S3 Inventory and Select:** Utilize S3 Inventory to get a list of objects and S3 Select to filter objects before applying batch operations, enhancing flexibility and control over data management tasks.

These capabilities streamline data management workflows and enable efficient batch processing of objects within Amazon S3, enhancing overall operational efficiency and scalability.

# S3 Storage Lens

S3 Storage Lens provides comprehensive insights and analytics to understand, analyze, and optimize storage across your entire AWS organization:

- **Default and Custom Dashboards:** Access default dashboards or create custom dashboards tailored to your specific needs and preferences.
- **Anomaly Detection:** Discover anomalies and unusual patterns in your storage usage, enabling proactive management and optimization of resources.
- **Metrics:** Storage Lens provides a wide range of metrics, including general insights about storage, storage bytes, object count, cost optimization metrics, data protection metrics, access management metrics, and event metrics.

By leveraging S3 Storage Lens, organizations can gain valuable insights into their storage usage, optimize costs, improve data protection, and enhance overall storage management practices across their AWS environment.
