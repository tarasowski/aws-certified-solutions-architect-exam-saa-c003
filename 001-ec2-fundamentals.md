# EC2 Fundamentals

# Introduction EC2

Amazon Web Services (AWS) offers a wide range of EC2 instance types tailored to different workload requirements. Each instance type is categorized based on its class, generation, and size, providing users with flexibility and scalability. The m5.2xlarge instance type, for example, falls under the "general" class, fifth generation, and is of extra-large size within its class. 

**More Information:**

1. **Instance Class:**
   - **General Purpose (m):** Designed to provide a balance between compute, memory, and networking resources. Suitable for a variety of workloads, including web servers, small databases, and development environments.
   - **Compute Optimized (c):** Optimized for tasks requiring high computational power. Ideal for batch processing, media transcoding, dedicated game servers, and high-performance computing (HPC) applications.
   - **Memory Optimized (r):** Focused on delivering high memory capacity for memory-intensive applications such as large-scale databases, in-memory databases, and applications requiring real-time processing.
   - **Accelerated Computing (e.g., p, g, f, inf):** Designed to leverage specialized hardware accelerators such as GPUs, FPGAs, and inference chips for tasks like machine learning inference, graphics rendering, and video encoding.
   - **Storage Optimized (i, d, h, o, u, z):** Optimized for storage-intensive workloads, providing high disk I/O performance and storage capacity. Ideal for applications such as high-frequency online transaction processing (OLTP), NoSQL databases, and caching databases.

2. **Generation:**
   - The generation number indicates the iteration and improvements made over time by AWS in terms of performance, efficiency, and features. Higher generation numbers often signify advancements in hardware, networking, and virtualization technologies.

3. **Size:**
   - The size designation within an instance class denotes the specific resource allocation, including CPU cores, RAM, storage, and network capacity. Larger sizes typically offer higher performance and scalability but may incur higher costs.

Understanding the characteristics and capabilities of each EC2 instance type helps users select the most suitable option for their specific application requirements, ensuring optimal performance, cost-effectiveness, and scalability on the AWS cloud platform.


# Security Groups

Security groups are essential components of AWS's network security model. They act as virtual firewalls for EC2 instances, controlling inbound and outbound traffic based on defined rules. These rules are primarily allow rules, specifying which traffic is permitted. Security groups can reference each other or use IP addresses for defining access permissions.

Inbound traffic flows from the internet to the EC2 instances, while outbound traffic goes from the instances to the internet. By default, outbound traffic is allowed to the World Wide Web. Security groups can be attached to multiple instances, providing consistent security settings across them.

It's important to note that security groups are tied to specific Virtual Private Clouds (VPCs) within AWS and operate on a per-region basis. They resemble firewalls placed outside of EC2 instances, protecting them from unauthorized access and malicious activity.

In the event of a timeout, often indicative of a security group issue, all inbound traffic is blocked while all outbound traffic remains authorized. Specific rules, such as allowing SSH (port 22) for Linux instances and RDP (port 3389) for Windows instances, are common practices to enable necessary access while maintaining security protocols.

To extend this summary, we can delve into the flexibility and scalability of security groups within AWS. They offer granular control over network traffic, enabling administrators to tailor access permissions based on individual instance requirements or application needs. Additionally, security groups integrate seamlessly with other AWS services, facilitating secure communication between different components of a cloud infrastructure.

Furthermore, AWS provides additional layers of security beyond security groups, such as Network Access Control Lists (NACLs) and AWS Identity and Access Management (IAM), allowing for a comprehensive security strategy to protect cloud environments. Understanding and effectively configuring security groups is fundamental to ensuring the integrity and confidentiality of data hosted on AWS EC2 instances.

Security groups operate in a stateful manner, which means they automatically track the state of connections and allow return traffic for permitted inbound connections. This simplifies network administration by eliminating the need to define explicit rules for return traffic. For example, if an inbound rule allows traffic on port 80 for a web server, the security group automatically allows return traffic from that web server on established connections.

# EC2 Instance Purchasing Options

The purchasing options for EC2 instances provide flexibility and cost-effectiveness tailored to various workload requirements:

1. **On-Demand Instances**: Pay for compute capacity by the second, with the option for Linux or Windows instances billed by the second and other operating systems billed by the hour.

2. **Reserved Instances**:
   - Offers significant discounts (up to 72%) compared to On-Demand pricing, ideal for long-term workloads.
   - Reservation period options of 1 or 3 years, with upfront or no upfront payment choices.
   - Convertible Reserved Instances allow flexibility to change instance types.

3. **Savings Plans**:
   - Commit to a specific amount of usage for 1 or 3 years, suitable for steady, long-term workloads.
   - Locked to instance family and region, supporting different operating systems.

4. **Spot Instances**:
   - Provides steep discounts (up to 90%) compared to On-Demand pricing, suitable for short-term workloads.
   - Can be interrupted by AWS if the current spot price exceeds your bid.
   - Ideal for batch jobs, image processing, but not recommended for critical or persistent workloads like databases.

5. **Dedicated Hosts**:
   - Allows booking an entire physical server, offering control over instance placement.
   - Suitable for applications with specific licensing requirements.
   - Options for On-Demand or Reserved Instances for 1 or 3 years.

6. **Dedicated Instances**:
   - Provides hardware dedicated solely to your account, ensuring isolation from other customers within the same account.

7. **Capacity Reservations**:
   - Reserves capacity in a specific Availability Zone (AZ) for any duration, without billing discounts.
   - Useful for short-term workloads requiring uninterrupted access in specific AZs.

Additional features include:

- **EC2 Spot Instance Requests**:
   - Offers significant cost savings (up to 90% compared to On-Demand) by defining a maximum spot price.
   - Ideal for one-time or persistent requests, suitable for batch jobs and big data analysis.

- **Spot Fleets**:
   - Comprises a mix of Spot Instances and On-Demand Instances.
   - Designed to meet target capacity with price constraints, allowing flexibility in launch pools, instance types, operating systems, and AZs.
   - Various strategies available to optimize allocation, such as LowestPrice, Diversified, CapacityOptimized, and PriceCapacityOptimized.

Public vs. Private IP

Private vs. public IP addresses play crucial roles in networking, particularly within cloud environments like AWS. 

Private IPs are used for communication within a private network, typically within a company or a cloud-based environment like AWS. By attaching an internet gateway to a private network in AWS, instances within that network gain access to the internet. This allows them to communicate with resources outside of the private network, such as accessing web servers or external databases. It's like having a bridge between your private network and the vast expanse of the internet.

On the other hand, public IPs are used for communication over the internet. When you start an EC2 instance in AWS, it's assigned a public IP address. However, this IP address can change when you stop and start the instance unless you use an Elastic IP (EIP). Elastic IPs provide a static, fixed IP address for your instance, ensuring consistent accessibility even if the instance is stopped and restarted.

While Elastic IPs provide stability, they have limitations. AWS allows a maximum of five Elastic IPs per account and charges for them. It's generally advised to avoid using Elastic IPs due to their associated costs and architectural implications. Instead, best practices include using dynamic public IPs and associating them with DNS names for easier management. Alternatively, employing load balancers can help distribute traffic efficiently without relying on individual instance public IPs.

In summary, private IPs facilitate internal communication within a network, while public IPs enable communication over the internet. Elastic IPs provide static public IPs for AWS instances, but their usage should be carefully considered due to cost and architectural concerns. Utilizing dynamic public IPs with DNS or load balancers is often recommended for better scalability and cost-effectiveness.

# Placements Groups

Placement groups in Amazon EC2 offer control over instance placement strategies. They provide several types:

1. **Cluster Placement Group**: Designed for applications that require low-latency communication, it clusters instances within a single Availability Zone (AZ) to minimize network latency. While offering high performance, it also poses a higher risk due to potential correlated failures.

2. **Spread Placement Group**: This type spreads instances across distinct underlying hardware to minimize the risk of simultaneous failures. Limited to a maximum of seven instances per group per AZ, it's suitable for critical applications that require high availability.

3. **Partition Placement Group**: Ideal for distributed applications like Hadoop, Cassandra, or Kafka, it spreads instances across multiple partitions within an AZ. Each partition represents a distinct rack, enabling the scaling of hundreds of EC2 instances. It offers both performance and fault tolerance benefits.

By leveraging placement groups, users can tailor instance placement to their specific workload requirements, balancing performance, availability, and fault tolerance effectively.

Elastic Network Interface (ENI)

An Elastic Network Interface (ENI) serves as a virtual network card within an Amazon Virtual Private Cloud (VPC), acting as a logical component to facilitate connectivity for instances. It enables instances within a VPC to communicate with other AWS services like Amazon S3, RDS, or other instances within the same VPC by providing the necessary network interface. ENIs play a crucial role in instance failover scenarios, allowing them to detach from one instance and attach to another seamlessly, ensuring continuous connectivity and functionality. They essentially act as the bridge that allows instances to interact with the broader AWS ecosystem and maintain network connectivity even in dynamic environments.

You can think of an Elastic Network Interface (ENI) as similar to a physical network interface card (NIC) in a traditional computer. Just like a NIC facilitates network connectivity for a physical machine, an ENI serves as the virtual equivalent within an Amazon Virtual Private Cloud (VPC). It provides the necessary networking capabilities for instances in the VPC to communicate with each other, as well as with other AWS services and resources outside the VPC. So, in essence, an ENI functions as a virtual network card, enabling instances to send and receive network traffic within the AWS environment.

# EC2 Hibernate

EC2 Hibernate is a feature that allows users to preserve the in-memory state of their instances while stopping them. This enables much faster boot times when compared to traditional stop and start methods. When an instance is hibernated, its RAM state is written to a file in the root EBS volume, which must be encrypted. This allows users to load the RAM and resume the instance without it ever being fully stopped. However, hibernated instances can't remain in this state for more than 60 days, and all data stored in RAM is ultimately stored on the EBS disk.

EC2 Hibernate offers significant advantages for users who need to quickly stop and start instances while preserving their current state. By saving the RAM state to the encrypted root EBS volume, users can achieve faster reboots without needing to reload data from external sources. This feature is particularly useful for applications with large datasets or complex configurations that would benefit from reduced downtime and faster recovery times.
