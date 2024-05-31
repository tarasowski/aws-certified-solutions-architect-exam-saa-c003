# RDS + Aurora + Elasticache

# Amazon RDS
**Amazon RDS Summary:**

- **Managed Database Service:** Amazon RDS (Relational Database Service) is a managed database service provided by AWS.
  
- **Supported Databases:** RDS supports various database engines including PostgreSQL, MariaDB, Oracle, Aurora, and MySQL.

- **Managed by AWS:** AWS handles the management tasks such as backups, patching, and scaling for the databases.

- **Benefits of RDS:** Using RDS relieves users from the burden of managing databases themselves, allowing them to focus on application development.

- **Monitoring Dashboards:** RDS provides monitoring dashboards to track performance metrics and ensure the health of databases.

- **Multi-AZ for Disaster Recovery:** RDS offers Multi-AZ (Availability Zone) deployment for automatic failover in case of a disaster, ensuring high availability.

- **Scaling:** RDS allows for both vertical scaling (increasing the resources of a single instance) and horizontal scaling (adding more instances) to accommodate changing workload demands.

- **SSH Limitation:** Users cannot directly SSH into the RDS instances as AWS manages the underlying infrastructure. Access is typically managed through database connection endpoints provided by AWS.

This summary highlights the key features and advantages of Amazon RDS as a managed database service offered by AWS.

# RDS Storage Auto Scaling
**RDS Storage Auto Scaling Summary:**

- **Dynamic Storage Increase:** RDS Storage Auto Scaling allows for automatic and dynamic scaling of storage on your RDS DB instance.

- **Triggered by Low Storage:** When RDS detects that the available free database storage is running low, it automatically scales up the storage capacity.

- **Threshold Setting:** Users can define a maximum storage threshold to control the scaling behavior and prevent unexpected costs.

- **Conditions for Scaling:** Scaling occurs when the free storage falls below 10% of the allocated storage capacity, and this low-storage condition persists for at least 5 minutes.

- **Modification Interval:** Additionally, scaling will only occur if at least 6 hours have passed since the last storage modification, preventing frequent and unnecessary scaling operations.

This feature of RDS enables automated and efficient management of storage resources, ensuring that database instances have sufficient storage capacity to meet operational needs without manual intervention.

# RDS Read Replicas vs. Multi-AZ

**RDS Read Replicas vs. Multi-AZ Summary:**

- **Scaling Reads:** Both RDS Read Replicas and Multi-AZ configurations help in scaling reads, but they have different approaches.
  
- **Read Replicas:** 
  - Users can create up to 15 read replicas.
  - Replicas can be set up in the same Availability Zone (AZ), across AZs, or even across regions for disaster recovery.
  - Replication occurs asynchronously, meaning reads are eventually consistent.
  - Replicas can be promoted to become their own standalone database.
  - Applications need to manage connection strings to utilize all read replicas.
  - Read replicas are typically used for read-heavy workloads like analytics.
  - Read replicas support only read operations (e.g., SELECT queries), not write operations like INSERT, UPDATE, or DELETE.

- **Multi-AZ:** 
  - Multi-AZ configurations provide high availability by maintaining a standby replica in a different AZ.
  - Failover to the standby replica occurs automatically in case of primary instance failure.
  - Multi-AZ configurations are primarily for ensuring high availability rather than scaling reads.

Both options offer distinct advantages and are suitable for different scenarios based on the specific requirements of scalability and availability.


# RDS Read Replicas - Network Costs

**RDS Read Replicas - Network Costs Summary:**

- **Intra-AZ Traffic:** Traffic between Availability Zones (AZs) within the same region is not subject to additional charges.
  
- **Inter-Region Traffic:** However, when data replication occurs across different regions, network costs may apply.
  
- **Cost Consideration:** Cross-region traffic for replication purposes may incur additional fees, contrasting with the cost-free traffic within the same region.

Understanding the network cost implications is crucial for optimizing expenses when utilizing RDS Read Replicas, especially when replication spans multiple regions.


# RDS Multi AZ (Disaster Recovery)
**RDS Multi AZ (Disaster Recovery) Summary:**

- **SYNC Replication:** Multi-AZ configurations utilize synchronous replication to ensure data consistency between the primary and standby databases.

- **Automatic Failover:** A single DNS name is provided, enabling automatic application failover to the standby database in the event of a primary database failure.

- **Increased Availability:** Multi-AZ setups enhance availability by maintaining a synchronized standby database in a different Availability Zone (AZ).

- **Failover Triggers:** Failover can be triggered by various events including AZ loss, network issues, instance failure, or storage failure affecting the primary database.

- **Promotion to Master:** In case of a failover event, the standby database is promoted as the new master to resume database operations.

- **Dedicated for Failover:** The standby database serves the sole purpose of facilitating failover, ensuring continuity of operations during primary database disruptions.

- **Disaster Recovery with Read Replicas:** Read replicas can be configured as Multi-AZ setups to extend disaster recovery capabilities, further bolstering redundancy and resilience.

RDS Multi AZ configurations offer robust disaster recovery solutions by providing synchronized standby databases for automatic failover, bolstering application resilience against various failure scenarios.


# RDS from singel AZ to multi AZ
**RDS from Single AZ to Multi AZ Summary:**

- **Zero Downtime Operation:** Transitioning from a single Availability Zone (AZ) to a Multi-AZ configuration in RDS is seamlessly executed without requiring database downtime.

- **Simple Modification Process:** The migration process involves initiating a modification request for the database, which can be achieved through a simple click.

- **Internal Operations:** Behind the scenes, the following steps are performed:
  - A snapshot of the existing database is taken to capture its current state.
  - A new database instance is restored from the snapshot in a different AZ.
  - Synchronization mechanisms are established between the original and newly created databases to ensure data consistency.

This migration process enables users to enhance the availability and fault tolerance of their RDS instances without interrupting ongoing operations, ensuring continuity of service during the transition.


# RDS Custom
**RDS Custom Summary:**

- **Supported Databases:** RDS Custom is available exclusively for Oracle and Microsoft SQL Server databases.

- **SSH Access:** Unlike standard RDS instances, users can SSH into the instance, granting them direct access to the underlying operating system and database.

- **Full Configuration Control:** Users have full administrative access and can configure settings, apply patches, and enable/disable features according to their requirements.

- **Admin Access to OS and Database:** With RDS Custom, users gain unrestricted administrative access to both the underlying operating system and the database, allowing for extensive customization and management capabilities.

RDS Custom provides advanced users with unparalleled flexibility and control over their Oracle and Microsoft SQL Server databases, empowering them to tailor the environment to their exact specifications.


# Amazon Aurora

**Amazon Aurora Summary:**

- **Proprietary Database:** Amazon Aurora is a proprietary database offered by AWS, compatible with MySQL and PostgreSQL.

- **Performance Optimization:** Aurora boasts significant performance enhancements, providing up to 5 times better performance than MySQL and 3 times better performance than PostgreSQL.

- **Automated Storage Scaling:** Storage capacity in Aurora automatically scales in increments of 10GB, with a maximum limit of 128TB.

- **Fast Replication:** Aurora supports up to 15 replicas, with replication processes significantly faster than MySQL (10ms replica lag).

- **Instantaneous Failover:** Failover in Aurora is instantaneous, ensuring minimal downtime in case of primary instance failure.

- **Cost and Efficiency:** While Aurora may cost more than traditional RDS, its efficiency and performance gains often justify the expense (approximately 20% more expensive).

- **Master-Replica Architecture:** Aurora operates with one master instance and supports up to 15 read replicas, with the ability to promote a replica to a master if needed.

- **Cross-Region Replication:** Aurora enables cross-region replication, facilitating disaster recovery and data locality requirements.

- **Data Redundancy:** Data in Aurora is automatically replicated across three Availability Zones (AZs), with six copies maintained for redundancy.

- **Automated Failover:** Automated failover for the master instance occurs in less than 30 seconds, ensuring high availability and reliability.

Amazon Aurora offers a highly efficient and scalable database solution, ideal for demanding workloads that require superior performance, availability, and scalability.


# Aurora DB Cluster

**Aurora DB Cluster Summary:**

- **Connection-Level Load Balancing:** Load balancing within Aurora DB clusters occurs at the connection level, facilitated by reader endpoints.

- **Single Writer Endpoint:** A single writer endpoint is provided for interactions with the master instance within the cluster.

- **Auto-Scaling Reader Endpoints:** Reader endpoints, responsible for distributing read traffic across replicas, are automatically scaled up as required.

- **Automated Patching:** Aurora enables automated patching with zero downtime, ensuring that patches and updates are applied seamlessly without interrupting database operations.

- **Push-Button Scaling:** Scaling operations within Aurora are simplified with push-button scalability, allowing users to easily adjust resources to accommodate changing workload demands.

- **Backtrack Feature:** Aurora introduces the backtrack feature, enabling users to restore data to any point in time without relying on traditional backups. This feature provides unparalleled flexibility by allowing users to backtrack and restore data at any desired moment without the need for prior backups.

The Aurora DB cluster offers advanced features and capabilities designed to enhance scalability, availability, and data management, making it a powerful choice for demanding database workloads.

# Aurora Replicas - Auto Scaling

**Aurora Replicas - Auto Scaling Summary:**

- **Dynamic Scaling:** Aurora Replicas can be configured for auto scaling based on CPU usage. When CPU usage increases, additional replicas are automatically provisioned to handle the load, ensuring optimal performance without manual intervention.

- **Reader Endpoint Usage:** Auto scaling occurs seamlessly behind the scenes, and users can continue to utilize the reader endpoint for accessing read replicas without the need for manual adjustment.

**Aurora Custom Endpoints Summary:**

- **Custom Endpoint Definition:** Users can define subsets of Aurora instances as custom endpoints, allowing for targeted access to specific instances within the Aurora cluster.

- **Analytics Optimization:** Custom endpoints are particularly useful for running analytics queries on larger instances optimized for such tasks, enhancing performance and efficiency.

- **Reduced Reliance on Reader Endpoint:** Once custom endpoints are defined, the reliance on the general reader endpoint diminishes, as users can set up multiple custom endpoints tailored for different tasks, streamlining access and optimizing resource utilization.

# Aurora Serverless

- **On-Demand Capacity:** Aurora Serverless eliminates the need for provisioned capacity in advance, allowing resources to scale automatically based on actual usage.


- **Automatic Scaling:** Resources in Aurora Serverless scale automatically based on demand, ensuring optimal performance and cost-effectiveness without manual intervention.

- **Cost Efficiency:** With no provisioned capacity, users only pay for the resources consumed, leading to cost savings during periods of low utilization.

- **Pause and Resume:** Aurora Serverless allows databases to pause during periods of inactivity, reducing costs further, and automatically resumes when activity resumes.

- **Built-in Monitoring:** Monitoring tools provide insights into database performance and usage, enabling optimization of resource allocation.

- **High Availability:** Aurora Serverless offers built-in high availability, ensuring database reliability with automated failover and data replication across multiple Availability Zones.

- **Compatibility:** Aurora Serverless is compatible with MySQL and PostgreSQL, offering flexibility for a wide range of applications and workloads.

Global Aurora
**Global Aurora Summary:**

- **Cross-Region Read Replicas:** Global Aurora enables the creation of read replicas across multiple regions, facilitating disaster recovery and providing low-latency access to data.

- **Disaster Recovery:** Global Aurora serves as an effective disaster recovery solution, ensuring data availability and minimizing downtime in the event of regional outages or disasters.

- **Global Database:** It offers a global database architecture where one region serves as the primary (read/write) region, while up to five secondary regions provide read-only access.

- **Replication Efficiency:** Replication lag between the primary and secondary regions is minimal, typically less than one second, ensuring data consistency across the globe.

- **Scalability:** Each secondary region supports up to 16 read replicas, allowing for scalable read operations and efficient distribution of read traffic.

- **RTO < 1 Minute:** With rapid failover capabilities, Global Aurora ensures a recovery time objective (RTO) of less than one minute, minimizing the impact of regional failures.

- **Fast Replication:** Cross-region replication is highly efficient, with typical replication times of less than one second, enabling near-real-time data synchronization across regions.


# Aurora Machine Learning

**Aurora Machine Learning Summary:**

- **SQL-Based Predictions:** Aurora Machine Learning enables the integration of machine learning (ML) predictions directly into SQL queries, simplifying the process of incorporating ML insights into database operations.

- **Integration with SageMaker:** It seamlessly integrates with Amazon SageMaker, a comprehensive ML service, allowing users to leverage SageMaker's capabilities for training and deploying ML models.

- **Integration with Comprehend:** Aurora Machine Learning also integrates with Amazon Comprehend, a natural language processing (NLP) service, enabling sentiment analysis and other text-based ML tasks directly within the database.

- **No ML Experience Required:** Users can harness the power of ML without needing extensive ML expertise, as Aurora Machine Learning abstracts away much of the complexity involved in ML model development and deployment.

- **Use Cases:** This integration empowers businesses to leverage ML for various tasks such as fraud detection, targeted advertising, sentiment analysis, and product recommendations, enhancing decision-making and user experiences.

# RDS Backups

**RDS Backups Summary:**

- **Automatic Backup Expiry:** Automatic backups have an expiration period, typically ranging from 1 to 35 days, depending on the retention policy set by the user.

- **Manual Backup Persistence:** Unlike automatic backups, manual backups do not expire and persist until explicitly deleted by the user.

- **Cost-Saving Strategy:** To save costs, users can delete the RDS database, create a manual snapshot, store the snapshot separately, and later restore the database from the snapshot as needed.

- **Automatic Backup Frequency:** Automatic backups are taken daily and retain transaction logs every 5 minutes, allowing for point-in-time recovery from the oldest available backup to as recent as 5 minutes ago.

- **Retention Period:** Users can specify a retention period for automatic backups, ranging from 1 to 35 days, or set it to 0 to disable automatic backups entirely, although this is not recommended for production databases.


# Aurora Backups

**Aurora Backups Summary:**

- **Automatic Backup:** Aurora does not allow the disabling of automated backups. These backups are retained for a duration ranging from 1 to 35 days, facilitating point-in-time recovery within that timeframe.

- **Point-in-Time Recovery:** Users can recover their Aurora databases to any point within the retention period of the automated backups, providing flexibility in data restoration.

- **Manual Database Snapshots:** Users can also create manual database snapshots at any time, providing an additional layer of backup and recovery options. These snapshots persist until explicitly deleted by the user.

Aurora's backup system ensures data durability and provides users with various options for backup and recovery, including both automated backups and manual snapshots.


# Restore RDS & Aurora

**Restore RDS & Aurora Summary:**

- **Restoring RDS Backup or Snapshot:**
  - Restoring a backup or snapshot in RDS creates a new database instance based on the backed-up data.
  - For MySQL RDS databases, backups stored in Amazon S3 can be restored to create a new database instance.

- **Restore from On-Premises to RDS:**
  - To migrate an on-premises database to RDS, create a backup of the on-premises database.
  - Store the backup file on Amazon S3.
  - Restore the backup file onto a new RDS instance running MySQL.

- **Restore MySQL Aurora Cluster from S3:**
  - To restore a MySQL Aurora cluster from S3:
  - Create a backup of the on-premises database using Percona XtraBackup or similar tool.
  - Store the backup file on Amazon S3.
  - Restore the backup file onto a new Aurora cluster running MySQL.

These restore processes allow for seamless migration and recovery of databases between different environments, leveraging the flexibility and scalability of AWS services like RDS and Aurora, along with the durability and accessibility of Amazon S3 storage.


# Aurora Database Cloning

**Aurora Database Cloning Summary:**

- **Purpose:** Aurora database cloning enables the replication of a production database for use in staging or testing environments.

- **Process:** To clone a database:
  - Create a new Aurora DB cluster from an existing one, specifying the source cluster.
  - The cloning process is rapid and typically faster than snapshot-based restoration.
  - It employs a copy-on-write protocol, minimizing data duplication and ensuring efficient resource utilization.

- **Speed and Efficiency:** Database cloning in Aurora is known for its speed and cost-effectiveness, making it an ideal solution for replicating databases across environments without incurring significant time or resource overheads.

This feature streamlines the process of creating staging or testing environments by providing a quick and efficient method for replicating production databases.

# RDS Security

**RDS Security Summary:**

- **At-Rest Encryption:**
  - RDS provides at-rest encryption for data stored in the database.
  - Encryption keys are managed using AWS Key Management Service (KMS).

- **Encryption for Master and Replicas:**
  - Both the database master and replicas can be encrypted using AWS KMS.
  - If the master database is not encrypted, read replicas cannot be encrypted either.

- **Encryption Conversion:**
  - To encrypt an unencrypted database, one method is to create a snapshot of the unencrypted database and restore it as an encrypted database.

- **IAM Authentication:**
  - RDS supports IAM authentication, allowing users to connect to the database using IAM roles instead of traditional username/password authentication.
  - This enhances security by leveraging AWS IAM for database access control, providing an additional layer of authentication and authorization.

RDS offers robust security features to protect data both at rest and in transit, including encryption and IAM authentication, ensuring the confidentiality and integrity of databases hosted on the platform.


# RDS Proxy

**RDS Proxy Summary:**

- **Purpose of Proxy:**
  - RDS Proxy acts as an intermediary between applications and RDS databases, addressing challenges associated with managing database connections.

- **Connection Pooling:**
  - RDS Proxy pools and shares connections established with the database, optimizing resource utilization and enhancing scalability.

- **Resource Efficiency:**
  - By efficiently managing connections, RDS Proxy improves CPU, RAM, and database resource utilization, leading to better performance and cost efficiency.

- **Serverless Deployment:**
  - RDS Proxy operates in a fully serverless manner, eliminating the need for manual management of infrastructure.

- **High Availability:**
  - RDS Proxy is available across multiple Availability Zones (AZs), ensuring high availability and fault tolerance.

- **Reduced Failover Time:**
  - It reduces RDS failover time by up to 66%, minimizing downtime and improving application resilience.

- **Compatibility and Security:**
  - RDS Proxy supports various RDS database engines including MySQL, PostgreSQL, MariaDB, and Microsoft SQL Server.
  - It enforces IAM authentication for database access and securely stores credentials in AWS Secrets Manager.

- **Network Isolation:**
  - RDS Proxy is never publicly available and must be accessed from within a Virtual Private Cloud (VPC), ensuring network isolation and security.

- **Usage with Lambda Functions:**
  - When connecting to the database via Lambda functions, using RDS Proxy is recommended over direct connections to manage potentially large numbers of concurrent connections more efficiently.

RDS Proxy simplifies and optimizes database connectivity for applications, improving scalability, reliability, and security while reducing management overhead.


# ElastiCache

**ElastiCache Summary:**

- **Managed Redis or Memcached:**
  - ElastiCache is a managed service provided by AWS for deploying and managing Redis or Memcached clusters.

- **In-Memory Database with High Performance:**
  - ElastiCache operates as an in-memory database, offering exceptionally high performance for caching frequently accessed data.

- **Stateless Application:**
  - Utilizing ElastiCache can help in making applications stateless by offloading session data or other frequently accessed data to the cache.

- **Cache Hit and Miss Strategy:**
  - Applications query ElastiCache first; if there's a cache hit, data is retrieved from the cache, otherwise, data is fetched from the primary data source such as RDS.

- **Built-in Cache Invalidation:**
  - ElastiCache provides built-in mechanisms for cache invalidation, ensuring that cached data remains accurate and up-to-date.

- **Support for Session Data:**
  - ElastiCache can be used to store session data, enabling stateless application architecture while maintaining session persistence.

- **Redis Features:**
  - Redis in ElastiCache offers features like Multi-AZ with auto-failover, read replicas, data durability, and support for advanced data structures like sets and sorted sets.

- **Memcached Features:**
  - Memcached in ElastiCache supports multi-node sharding for scaling, but it lacks high availability, persistence, backup and restore capabilities, and is designed with a multi-threaded architecture.

ElastiCache provides a scalable and highly performant caching solution, whether using Redis or Memcached, suitable for a variety of use cases such as caching, session storage, and improving application performance.

# ElastiCache Security

**ElastiCache Security Summary:**

- **IAM Authentication (Redis Only):**
  - ElastiCache supports IAM authentication for Redis clusters, allowing users to use IAM roles for authentication and authorization purposes.

- **IAM Usage Clarification:**
  - IAM authentication in ElastiCache is primarily used for AWS API-level security, providing fine-grained access control over AWS resources, including ElastiCache clusters.

- **Redis Authentication:**
  - Redis clusters in ElastiCache support additional authentication mechanisms such as password/token authentication and SSL/TLS encryption for data in transit, enhancing data security.

- **Memcached Authentication:**
  - For Memcached clusters, authentication is supported through SASL-based mechanisms, ensuring secure access control to the cache nodes.

ElastiCache offers various authentication options and encryption features to secure data access and communication, enhancing the overall security posture of cached data in Redis and Memcached clusters.

# Patterns for ElastiCache

**Patterns for ElastiCache Summary:**

- **Lazy Loading:**
  - In this pattern, all read data is cached upon retrieval from the database. However, data in the cache can become stale over time as it is not automatically updated when changes occur in the database.

- **Write-Through:**
  - Write-through caching involves adding or updating data in the cache simultaneously when it is written to the database. This ensures that the cache remains synchronized with the database, reducing the likelihood of stale data.

- **Session Store:**
  - ElastiCache can be used as a session store to store temporary session data, such as user sessions in web applications. This pattern often utilizes the Time-to-Live (TTL) feature of ElastiCache to automatically expire session data after a specified period, ensuring data freshness and efficient resource utilization.

These patterns demonstrate the versatility of ElastiCache in supporting various caching strategies to improve application performance and scalability while ensuring data consistency and reliability.
ElastiCache Redis Use Case
- gaming leaderboard are computationally complex
- redis sorted sets guarantee both uniqueness and elment ordering
- each time a new element added it is ranked in a realtime, then added in correct order

**RDS Database Ports:**

- **PostgreSQL:** 5432
- **MySQL:** 3306
- **Oracle RDS:** 1521
- **Microsoft SQL Server:** 1433
- **MariaDB:** 3306 (same as MySQL)
- **Aurora:** 
  - PostgreSQL compatible: 5432
  - MySQL compatible: 3306

These port numbers are default configurations for accessing the respective RDS database engines.
