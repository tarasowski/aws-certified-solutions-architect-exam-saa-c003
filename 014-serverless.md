# Serverless

# AWS Lambda
**AWS Lambda** is a serverless compute service that allows you to run code without provisioning or managing servers. Here are some key points about Lambda:

1. **Pay-Per-Use Model**:
   - You are charged based on the number of requests and the duration of your code's execution.
   - Pay-per-request and compute time increments, with no charge when your code is not running.

2. **Resource Configuration**:
   - Supports up to 10GB of memory per function, with memory increments of 1MB.
   - Allows custom runtime APIs, enabling the execution of code written in any programming language.
   - Supports Lambda container images, but requires implementing a custom runtime for Lambda.

3. **Use Cases**:
   - Well-suited for various tasks such as data processing, real-time file processing, API backend services, and more.
   - Offers flexibility for running code in response to events triggered by other AWS services or HTTP requests.

4. **Limits per Region**:
   - Defines various limits per region, including memory, execution duration, environment variables, disk space, concurrency, and function size.
   - Allows the use of the `/tmp` directory to load additional files during function startup.

5. **AWS Lambda SnapStart**:
   - Enhances Lambda function performance by up to 10x for Java 11 and above.
   - Achieved by invoking Lambda functions from a pre-initialized state, reducing cold start times.
   - Takes snapshots of Lambda function states, allowing new invocations to start from these snapshots.


# AWS Lambda@Edge / Cloudfront Functions

**AWS Lambda@Edge / CloudFront Functions** allow you to customize content delivery through Amazon CloudFront, providing powerful capabilities to modify viewer requests and responses. Here's a breakdown of both CloudFront Functions and Lambda@Edge:

### CloudFront Functions:
- **Lightweight Functions**:
  - Written in JavaScript.
  - Used to modify viewer requests and viewer responses.
- **Request and Response Phases**:
  - Operate in two phases: viewer request and viewer response.
  - Viewer Request: After CloudFront receives a request from a viewer.
  - Viewer Response: Before CloudFront forwards the response to the viewer.
- **Native Integration**:
  - A native feature of CloudFront, allowing you to manage code entirely within CloudFront.
- **Limitations**:
  - Maximum package size of 2MB.
  - Maximum execution time of less than 1ms.
  - No network access.
  - Package size limited to 10KB.
  - No access to the request body.
- **Use Cases**:
  - Cache key normalization.
  - Header manipulation.
  - URL rewrites or redirects.
  - Request authentication and authorization.

### Lambda@Edge:
- **Programming Languages**:
  - Supports Node.js and Python.
- **Scalability**:
  - Scales to thousands of requests per second.
- **Execution Phases**:
  - Operates in four phases: viewer request, origin request, origin response, and viewer response.
- **Regional Deployment**:
  - Deployed in one AWS region (typically us-east-1), then replicated to CloudFront edge locations.
- **Limits**:
  - Maximum execution time ranging from 5 to 10 seconds.
  - Memory limits range from 128MB to 10GB.
  - Package size limits range from 1MB to 50MB.
- **Use Cases**:
  - Tasks requiring longer execution times.
  - Utilizing CPU or memory-intensive operations.
  - Using third-party libraries.
  - Network access to external services.
  - File system access or access to the body of HTTP requests.

Both CloudFront Functions and Lambda@Edge provide powerful capabilities for customizing content delivery, allowing you to tailor your CDN behavior to specific requirements and enhance the performance and security of your applications.


# AWS Lambda in VPC

**AWS Lambda in VPC:**

By default, Lambda functions are executed outside of your Virtual Private Cloud (VPC), in an AWS-owned VPC. Consequently, they lack direct access to resources within your VPC, such as RDS, ElastiCache, or internal ELBs.

**Launching Lambda in VPC:**
- To enable Lambda to access resources in your VPC, you must specify:
  - VPC ID
  - Subnets
  - Security groups
- Lambda creates an Elastic Network Interface (ENI) within your specified subnets.
- Example flow: Lambda function -> Private subnet -> ENI -> RDS inside the VPC

**Lambda with RDS Proxy:**
- RDS Proxy enhances scalability by pooling and sharing database connections.
- Improves availability by reducing failover time by up to 66% and preserving connections.
- Enhances security by enforcing IAM authentication and storing credentials in Secrets Manager.
- Lambda functions utilizing RDS Proxy must be deployed within your VPC, as RDS Proxy is never publicly accessible.

Integrating Lambda with your VPC and utilizing RDS Proxy can significantly enhance the performance, scalability, and security of your serverless applications, particularly when interacting with relational databases like RDS.


**RDS Invoking Lambda & Event Notification:**

**Invoke Lambda from within your DB Instance:**
- Supported by RDS for PostgreSQL and Aurora MySQL.
- Use cases include sending welcome emails or performing other automated tasks based on database events.
- Requires allowing outbound traffic from your DB instance to your Lambda function.
- Provides a seamless way to trigger Lambda functions directly from database events, enhancing automation and real-time responsiveness.

**RDS Event Notifications (not invoking Lambda):**
- Provides event notifications for various RDS events, such as DB instance creation, deletion, or snapshot creation.
- Does not provide information about the data itself; it's focused on notifying about administrative events.
- Offers near real-time notifications, typically delivered within up to 5 minutes.
- Useful for monitoring and managing RDS resources, allowing you to stay informed about changes and events related to your database instances and snapshots.

# DynamoDB
**DynamoDB:**

- Provides single-digit millisecond performance for both read and write operations, making it suitable for applications requiring low-latency access to data.
- Offers auto-scaling capabilities to manage throughput capacity automatically based on workload demands.
- Supports standard and infrequent access classes, allowing you to optimize costs based on the access patterns of your data.
- Attributes within items can be nullable, providing flexibility in data modeling.
- Maximum item size is 400 KB, ensuring efficient storage and retrieval of data.
- Supports various data types including scalar types, document types, and set types, catering to diverse data modeling needs.
- Offers a flexible schema that can rapidly evolve, making it suitable for agile development and better suited than traditional relational databases like RDS for certain use cases.

**Read/Write Capacity Modes:**

- **Provisioned Mode:** In this mode, you provision and pay for the desired Read Capacity Units (RCUs) and Write Capacity Units (WCUs) upfront. DynamoDB automatically adjusts capacity in response to your traffic patterns within the provisioned limits. This mode is suitable for predictable workloads where you can estimate your throughput requirements.
  
- **On-Demand Mode:** In this mode, there is no need to provision or manage capacity. You simply pay for the read and write requests your application makes. This mode is ideal for workloads with unpredictable or highly variable traffic patterns, as it allows you to scale seamlessly without worrying about capacity planning.

# DynamoDB Advanced Features

**DynamoDB Advanced Features:**

**DynamoDB Accelerator (DAX):**
- Fully managed in-memory cache for DynamoDB tables.
- Helps alleviate read latency by caching frequently accessed data.
- Provides microseconds latency for cached data, improving application performance.
- No need to modify application logic to leverage DAX.
- Default TTL (Time-to-Live) for cached items is 5 minutes.
- Ideal for individual object caching or caching query and scan results.

**Stream Processing:**
- Utilizes DynamoDB Streams or Kinesis Data Streams for real-time data processing.
- Commonly used for real-time user analytics and cross-region applications.
- DynamoDB Streams offer 24 hours retention for a limited number of consumers.
- Kinesis Data Streams offer up to 1 year of retention.

**Global Tables:**
- Replicates DynamoDB tables across multiple AWS regions.
- Supports two-way replication, allowing bidirectional data synchronization.
- Enables active-active replication, allowing your application to read and write to any replicated table.

**Time-to-Live (TTL):**
- Automatically deletes items from a table after a specified expiration timestamp.
- Useful for scenarios like web session handling, where sessions need to be kept for a certain duration and then automatically removed.

**Backups for Disaster Recovery:**
- Offers continuous backups using Point-in-Time Recovery (PITR) for the last 35 days.
- Allows point-in-time recovery to any time within the backup window, creating a new table with the recovered data.
- Supports on-demand backups for long-term retention until explicitly deleted, with no impact on performance or latency.
- Supports cross-region copying of backups.

**S3 Integration:**
- Enables exporting DynamoDB table data to S3, provided PITR is enabled.
- Supports exporting data for the last 35 days, allowing for data analysis.
- Export formats include DynamoDB JSON or Ion format.
- Supports importing data from S3 using CSV, DynamoDB JSON, or Ion format, without consuming any write capacity. Import errors are logged in CloudWatch Logs.

# API Gateway
**API Gateway:**

API Gateway enables the creation of RESTful APIs with various features:

- It serves as a proxy for requests to AWS Lambda, removing the need to manage infrastructure.
- Supports WebSocket protocol for real-time, bidirectional communication.
- Offers versioning capabilities to manage different versions of APIs.
- Facilitates handling of different environments such as production and development.
- Provides robust security features for authentication and authorization.
- Allows the creation of API keys for access control.
- Supports Swagger for API documentation.
- Provides caching of API responses for improved performance.
- Allows for the exposure of AWS Lambda functions, HTTP endpoints, or other AWS services like SQS.
- Supports exposing any AWS service to the outside world.
- Offers HTTP APIs, a simpler version of REST APIs.
- Endpoint types include edge-optimized (default for global clients), regional (for clients within the same region), or private APIs for use inside your VPC.

Security options include:
- IAM roles for internal applications.
- Cognito for user authentication.
- Custom authorization logic.
- Custom domain names with HTTPS security through integration with AWS Certificate Manager (ACM). Note that if using an edge-optimized endpoint, the certificate must be in the `us-east-1` region. For region endpoints, the certificate must be in the API Gateway region.
- Setup of CNAME or A-alias record in Route 53 for custom domain names.


# Step Functions

Step Functions allow the creation of serverless visual workflows to orchestrate AWS Lambda functions and other AWS services. Key features include:

- Building complex workflows within AWS by defining sequences, parallel tasks, conditions, timeouts, and error handling.
- Integration with various AWS services including EC2, ECS, and API Gateway, as well as on-premises systems through AWS services.
- Implementation of human approval features for workflows that require human intervention or decision-making.
- Use cases include order fulfillment, data processing, web applications, and any workflow that requires coordination between multiple tasks or services.


# AWS Cognito

AWS Cognito provides user identity management for web and mobile applications. It consists of two main components:

1. **User Pool:**
   - A serverless user database where user identities are stored.
   - Supports integration with social identity providers like Facebook, Google, etc.
   - Allows users to sign in to applications using username/password or social login.
   - Often integrated with application load balancers to verify user logins.

2. **Identity Pool:**
   - Provides temporary credentials to users for accessing AWS resources directly.
   - Allows web or mobile applications to access AWS services like S3 or DynamoDB on behalf of the user.
   - Useful when applications need to access AWS resources without going through an application load balancer.
   - Can be authenticated via user pools or other identity providers like Google.

AWS Cognito enables secure authentication and authorization mechanisms for applications, allowing users to interact with resources securely.
