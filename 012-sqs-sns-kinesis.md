# Decoupling Applications: SQS, SNS, Kinesis

# Introduction to Messaging

In the realm of distributed systems, messaging plays a crucial role in facilitating communication between various components or services. Here's a brief overview:

- **Need for Data Sharing:** Services often need to share data with each other, either synchronously or asynchronously, to enable effective coordination and collaboration within a system.

- **Sync vs. Async Communication:** Messaging can be categorized into synchronous communication, where applications communicate directly with each other, and asynchronous communication, where messages are exchanged through intermediary components like queues.

- **Decoupling Applications:** Asynchronous messaging, in particular, enables the decoupling of applications by allowing them to communicate indirectly through messages. This decoupling enhances system flexibility, scalability, and resilience.

# What is SQS (Simple Queue Service)

Amazon SQS is a fully managed message queuing service provided by AWS. Here are some key features and characteristics:

- **Producer-Consumer Model:** Producers send messages to SQS queues, and consumers poll these queues to retrieve and process messages.

- **Decoupling Applications:** SQS is designed to decouple the components of distributed applications, allowing them to operate independently and asynchronously.

- **Scalability:** SQS offers unlimited throughput and supports an unlimited number of messages in the queue, making it suitable for high-volume and distributed systems.

- **Message Retention:** Messages in SQS queues can remain in the queue for a configurable duration, with a default minimum retention period of 4 days and a maximum of 14 days.

- **Low Latency:** SQS provides low-latency message delivery, typically around 10 milliseconds for both message publishing and retrieval.

- **Message Size:** Each message in SQS can be up to 256 KB in size, accommodating various types of payloads.

- **Message Delivery Guarantees:** SQS provides at-least-once message delivery, meaning that messages may be delivered more than once but are never lost. However, there's no strict ordering guarantee (best-effort ordering).

- **Consumer Scalability:** Multiple consumers can simultaneously poll an SQS queue to process messages, allowing for horizontal scalability.

- **Auto Scaling Integration:** SQS can be integrated with AWS Auto Scaling to dynamically adjust the number of consumers based on queue metrics, such as the approximate number of messages.

- **Encryption:** SQS ensures encryption of messages in transit (in-flight encryption) and also supports client-side encryption for added security.

- **Access Policies:** Access to SQS queues is managed through access policies, allowing you to specify who can send messages to and receive messages from a queue. This is in addition to IAM-based authentication and authorization.

In summary, Amazon SQS provides a reliable, scalable, and fully managed messaging service that enables asynchronous communication between distributed components or services within AWS and beyond. Its features, such as scalability, low latency, and message retention, make it well-suited for building loosely coupled and resilient distributed systems.

# Message Visibility Timeout

- **Concept:** When a consumer polls a message from an SQS queue, that message becomes temporarily invisible to other consumers. This invisibility period is known as the message visibility timeout.
  
- **Default Timeout:** By default, the message visibility timeout is set to 30 seconds. During this period, the message is expected to be processed and deleted by the consumer.

- **Handling Message Processing Time:** If a message requires more time for processing than the default timeout allows, you can adjust the message visibility timeout accordingly.

- **Impact of Timeout Setting:** It's crucial to strike a balance with the timeout setting. Setting it too high may cause delays in processing messages, while setting it too low may result in messages being returned to the queue prematurely.

# SQS Long Polling

- **Purpose:** SQS Long Polling enhances the efficiency of message retrieval by allowing consumers to wait for messages to arrive if the queue is currently empty.

- **Configuration:** Consumers can specify a longer polling duration (1 to 20 seconds) when requesting messages from the queue.

- **Advantages:** Long polling reduces the number of empty responses from the queue, leading to lower costs and improved performance compared to short polling.

- **Queue-Level Setting:** Long polling can be enabled at the queue level, ensuring consistent behavior across all consumers.

# FIFO (First-In-First-Out) Queues

- **Ordering:** FIFO queues preserve the order in which messages are sent and received. Messages are processed in the exact order they are sent into the queue.

- **Throughput:** FIFO queues have limited throughput compared to standard queues, with a maximum rate of 300 messages per second (without batching) and 3000 messages per second (with batching).

- **Exactly-Once Processing:** FIFO queues offer exactly-once message processing, ensuring that duplicates are removed and each message is processed only once.

- **Naming Convention:** FIFO queues are identified by the `.fifo` suffix in their names.

- **Content-Based Deduplication:** FIFO queues support an option for content-based deduplication, where messages with identical content are automatically filtered to prevent duplicates.

# SQS + Auto Scaling Group

- **Scaling Based on Queue Size:** SQS can be integrated with AWS Auto Scaling to dynamically scale the number of consumers (instances) based on the number of messages in the queue.

- **Metric Monitoring:** The `ApproximateNumberOfMessages` metric from SQS can trigger alarms in CloudWatch, which, in turn, can initiate scaling actions on the Auto Scaling group.

- **Buffering for Database Writes:** SQS acts as a buffer between application components, such as frontend services and database writes. Requests are first sent to SQS, dequeued by the Auto Scaling group, and then processed and inserted into the database.

- **Decoupling Applications:** This architecture decouples different tiers of an application, ensuring smoother operation, improved fault tolerance, and easier scalability.


# Amazon SNS (Simple Notification Service)

Amazon SNS is a fully managed messaging service provided by AWS, facilitating the pub/sub (publish/subscribe) messaging pattern. Here's an overview:

- **Pub/Sub Model:** With SNS, an event producer (publisher) sends messages to a topic, and multiple event consumers (subscribers) can receive and process these messages.

- **Scalability:** SNS allows for highly scalable and distributed message publication to potentially thousands or millions of subscribers.

- **Subscription Limits:** Each topic can support up to 12 million subscriptions, and AWS accounts can create up to 100,000 topics.

- **Supported Protocols:** Subscribers can receive messages via various protocols, including email, SMS, HTTP/HTTPS, SQS, Lambda, Kinesis Firehose, and more.

- **Direct Publish for Mobile Apps:** SNS provides direct publish capabilities for mobile applications, enabling the delivery of push notifications to mobile devices via SDKs.

- **Encryption and Access Policies:** SNS ensures encryption of messages in transit and at rest. Access policies control who can publish messages to topics.

# SNS + SQS: Fan Out Pattern

- **Pattern Description:** The Fan Out pattern involves publishing a message to an SNS topic and delivering that message to all subscribed SQS queues.

- **Decoupled Architecture:** This pattern enables fully decoupled communication between publishers and subscribers, ensuring no data loss and allowing SQS to provide message persistence.

- **Benefits:** By leveraging SQS for message delivery, the Fan Out pattern enhances fault tolerance and scalability in distributed systems.

# S3 Events to Multiple Queues

- **Limitations:** While S3 events can trigger multiple actions, such as invoking Lambda functions or sending messages to SQS, each event type and prefix combination can only have one S3 event rule.

- **Solution:** To achieve multiple queue subscriptions from S3 events, you can route the events through SNS and then distribute them to the desired SQS queues via subscriptions.

# SNS FIFO (First-In-First-Out)

- **Similarity to SQS FIFO:** SNS FIFO queues offer similar features to SQS FIFO queues, including deduplication and message ordering based on message group ID.

- **Naming Convention:** FIFO topics must have names ending with `.fifo`, similar to FIFO queues in SQS.

# Message Filtering

- **Consumer-Specific Filtering:** SNS allows for message filtering based on consumer-specific criteria using filter policies defined in JSON format.

- **Customization:** By applying filter policies, subscribers can receive only the messages that meet specific conditions, improving message relevance and reducing unnecessary processing.

# Amazon Kinesis

Amazon Kinesis is a platform provided by AWS for collecting, processing, and analyzing streaming data in real-time. It consists of several components:

## Kinesis Data Streams

- **Purpose:** Capture, process, and store data streams in real-time.
- **Shards:** Data Streams are composed of multiple shards, which serve as the unit of capacity provisioning. You must provision shards in advance based on expected workload.
- **Producers:** Data producers, such as applications, clients, SDKs, or the Kinesis Agent, write records into Data Streams. Each record includes a partition key and a data blob.
- **Consumers:** Data Streams can be consumed by various services including SDKs, AWS Lambda, Kinesis Firehose, and Kinesis Analytics.
- **Record Attributes:** Each record includes a partition key, sequence number, and data blob.
- **Retention:** Data retention can be set between 1 to 365 days.
- **Replayability:** Data can be replayed from the stream.
- **Capacity Modes:** Supports provisioned mode where you pay per provisioned shard per hour, and on-demand mode with default capacity.
- **Security:** Deployed within a region, supports encryption and VPC integration.
- **Monitoring:** Activity can be monitored using AWS CloudTrail.
- **Reading Data:** Consumers can read records from the beginning (TRIM_HORIZON) or from a specific point (AFTER_SEQUENCE_NUMBER, AT_TIMESTAMP).

## Kinesis Data Firehose

- **Purpose:** Load data streams into AWS data stores.
- **Fully Managed:** Automatically scales to handle varying workloads and manages resources.
- **Direct Data Delivery:** Streams data directly to S3, Redshift, Elasticsearch, or Splunk without needing intermediate storage.
- **Serverless:** No need to manage resources or servers.
- **Data Transformation:** Supports data transformation using AWS Lambda before delivery to destinations.
- **Security:** Encrypted data delivery and supports VPC endpoint policies.

## Kinesis Data Analytics

- **Purpose:** Analyze streaming data using SQL queries.
- **Real-time Insights:** Perform analytics on live streaming data with SQL.
- **Integration:** Easily integrates with Kinesis Data Streams and Kinesis Data Firehose.
- **Automatic Scaling:** Automatically scales based on query complexity and volume of data.
- **Output Options:** Send results to various destinations including S3, Redshift, Elasticsearch, or Lambda functions.
- **Real-time Monitoring:** Monitor and visualize queries and performance metrics in real-time.

# Kinesis Data Firehose

Kinesis Data Firehose is a fully managed service that makes it easy to load streaming data into AWS data stores and analytics services. Here are the key features and capabilities:

- **Data Collection:** Firehose can ingest data from various sources, acting as a receiver for data producers.
- **Record Size:** Supports records up to 1 MB in size.
- **Data Transformation:** Allows for data transformation using AWS Lambda functions before delivering it to destinations. This enables data enrichment, filtering, and format conversion.
- **Destination Options:** Data can be seamlessly written to destinations without writing any code. Supported destinations include Amazon S3, Amazon Redshift (via S3), Amazon OpenSearch Service, and various third-party services like Datadog, Splunk, and New Relic. Additionally, it supports HTTP endpoints for custom destinations.
- **Handling Failed Data:** Firehose can automatically write all or failed data into Amazon S3 for troubleshooting and reprocessing.
- **Near-Real-Time Delivery:** Offers near-real-time data delivery with a buffer interval ranging from 0 to 900 seconds. You can specify a minimum buffer size of 1 MB. Data is delivered within a few seconds of being ingested.
- **Automatic Storage Management:** Firehose automatically manages the storage of data and doesn't support data replay since it doesn't store the data internally.
- **Data Transformation:** Supports transforming record format into Parquet or ORC for efficient storage and analytics.
- **Data Prefixing:** Allows adding prefixes to the delivered S3 objects for better organization and management.
- **Buffer Management:** Utilizes buffering to accumulate data before delivering it to the target destination. Data is delivered either when the buffer size reaches a specified threshold (e.g., 5 MB) or when the buffer interval expires (e.g., 5 minutes).
- **Compression:** Supports compression formats like gzip and snappy to reduce storage costs and improve data transfer efficiency.
- **Visibility Delay:** It may take some time (up to the buffer size or buffer interval) for data to become visible in the destination, depending on the buffer configuration.

Kinesis Data Firehose simplifies the process of loading streaming data into AWS services and third-party destinations, providing flexibility, scalability, and reliability without the need for managing infrastructure or writing custom code.

# Data Ordering Kinesis vs.  SQS Fifo
- **Data Ordering in Kinesis:** Achieved by using a partition key, ensuring that records with the same key are sent to the same shard. This allows for ordered processing of data within each shard. However, different shards may contain data in a different order. Each shard can be consumed by only one consumer, providing strong ordering guarantees within that shard.

- **Data Ordering in SQS FIFO:** Similar to Kinesis, ordering is achieved using a `group_id`. Messages with the same `group_id` are processed in order, while messages in different groups can be processed independently. SQS FIFO provides ordered message processing within a message group. However, unlike Kinesis, where each shard can only be consumed by one consumer, SQS FIFO allows multiple consumers to process messages from different groups concurrently.

In both cases, the use of partition keys (in Kinesis) and `group_id` (in SQS FIFO) ensures that related messages are processed in order, providing deterministic message ordering within their respective systems.

| Feature          | SQS                                          | SNS                                          | Kinesis                                      |
|------------------|----------------------------------------------|----------------------------------------------|----------------------------------------------|
| Data Movement    | Consumer pulls data                          | Pushes data to subscribers                   | Standard: Pull data, Enhanced Fan-Out: Push data |
| Data Persistence | Data is deleted after being consumed        | Data is not persisted                        | Data expires after a certain period of time   |
| Scalability      | Can have as many workers as needed           | Up to 12 million subscribers                 | Provisioned mode: Fixed capacity, On-demand mode: Autoscaling |
| Throughput       | No need to provision throughput              | No need to provision throughput              | Standard: 2MB per shard, Enhanced Fan-Out: 2MB per shard per consumer |
| Ordering         | FIFO provides ordering guarantee             | Not inherently ordered                       | Ordered at the shard level                    |
| Integration      | Integrates with SQS for fan-out              | -                                            | -                                            |
| Message Delay    | Individual message delay capability          | -                                            | -                                            |
| Topic Limit      | -                                            | Up to 100k topics                            | -                                            |
| Use Case         | -                                            | Pub/Sub model                               | Real-time big data, analytics, etc.          |


Here's the information presented in a structured manner:

**Amazon MQ**

- **Purpose**: 
  - Traditional applications running on-premises may use open protocols such as MQTT, AMQP, STOMP, OpenWire.
  - Instead of re-engineering the app to use SQS and SNS when migrating to the cloud, Amazon MQ can be used.
  
- **Service Type**: 
  - Managed message broker service.

- **Supported Protocols**:
  - Supports protocols like MQTT, AMQP, STOMP, OpenWire.

- **Scalability**:
  - Doesn't scale as much as SQS/SNS.

- **Deployment**:
  - Runs on servers.
  - Can run in multi-AZ with failover.

- **Features**:
  - Provides both queue features like SQS and topic features like SNS.
  
- **Failover**:
  - Failover mechanism via EFS (Elastic File System) that acts as backup for data.
  - EFS can be mounted to multi-AZs.

Amazon MQ is essentially a managed message broker service designed to facilitate the migration of traditional applications running on-premises to the cloud without the need for extensive re-engineering. It supports various open protocols commonly used in traditional setups and offers features similar to both SQS and SNS, making it a convenient choice for such migration scenarios. However, it's important to note that it may not scale as much as SQS and SNS, and it runs on servers rather than being fully serverless.
