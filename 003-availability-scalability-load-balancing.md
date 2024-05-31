# High Availability and Scalability ELB & ASG

# Horizontal vs. Vertical Scaling

Horizontal scaling involves adding more resources to a system by increasing the number of machines or instances, rather than upgrading existing ones. It's a fundamental concept in distributed systems, where tasks are divided among multiple machines for improved performance and reliability. For example, a web application experiencing increased traffic can horizontally scale by adding more virtual machines or containers in a cloud environment like AWS or Azure to handle the load. This approach allows for flexible and cost-effective scaling as demand fluctuates.

High availability ensures that a system remains operational and accessible even in the face of failures or disruptions. It's often achieved by deploying resources across multiple Availability Zones (AZs) within a cloud region. For instance, a database deployed in two AZs ensures that if one zone experiences an outage, the system can continue to function without interruption from the other zone.

Vertical scaling involves increasing or decreasing the resources allocated to a single machine or instance. Scaling up involves adding more RAM, CPU, or storage capacity to an existing machine, while scaling down involves reducing these resources. For example, a database server may be vertically scaled up by adding more memory to improve query performance.

Horizontal scaling, on the other hand, focuses on adding more instances or machines to distribute the workload. Scaling out involves adding more instances to handle increased demand, while scaling in involves reducing the number of instances during periods of lower activity. This can be automated using auto-scaling mechanisms and load balancers, which dynamically adjust the number of instances based on factors like traffic patterns or resource utilization. For instance, an e-commerce website may automatically scale out during peak shopping hours to handle increased traffic and scale in during off-peak hours to save costs.

# Load Balancing

Load balancing, facilitated by Elastic Load Balancer (ELB) services, distributes incoming traffic across multiple EC2 instances, ensuring efficient resource utilization and high availability. ELB serves as a single point of access (DNS) for clients, spreading the load across various instances, all while conducting regular health checks to ensure optimal performance. Additionally, ELB provides SSL termination for secure HTTPS connections and can enforce stickiness using cookies. Managed by AWS, ELB handles upgrades and offers cost-effectiveness compared to self-managed solutions.

There are several types of ELBs to suit different needs: the Classic Load Balancer (now considered old), Application Load Balancer (ALB) for HTTP traffic, Network Load Balancer (NLB) for TCP and UDP traffic, and Gateway Load Balancer for IP protocol traffic analysis, like for firewalls.

ALB, operating at Layer 7, excels in HTTP traffic management, supporting features like WebSocket, routing tables for directing traffic to specific target groups based on paths, hostnames, or query strings. It's an ideal fit for microservices and container-based applications and offers port mapping capabilities to redirect to dynamic ports.

Target groups, utilized by ALB, are versatile, supporting EC2 instances, ECS tasks, Lambda functions, and private IP addresses. They enable health checks at the target group level and maintain fixed hostnames with ALB, while also allowing client IP forwarding.

NLB operates at Layer 4, handling TCP and UDP traffic with extreme performance, supporting millions of requests per second with lower latency compared to ALB. Each Availability Zone (AZ) can have a static IP, and NLB can assign Elastic IPs, making it suitable for high-performance requirements. It's commonly used alongside ALB, directing traffic to it with fixed IP addresses while allowing ALB to handle routing. NLB conducts health checks for TCP, HTTP, and HTTPS protocols, ensuring robustness and reliability.

To interact with the network at Layer 4, you typically use networking libraries or frameworks provided by your programming language or platform. Here's a general overview of how you could send packets from Layer 4 in an application:

1. **Select a Programming Language/Framework**: Choose a programming language or framework that provides networking capabilities. Popular choices include Python with libraries like `socket`, Java with `java.net`, or C/C++ with `sockets` or platform-specific APIs.

2. **Open a Socket**: In your application code, open a TCP socket. A socket represents an endpoint for communication between two machines over a network. You specify the IP address and port number to which you want to connect.

3. **Establish Connection**: Use the socket to establish a connection to the destination server. If you're building a client application, you would typically use the `connect()` function or method provided by your chosen networking library.

4. **Send Data**: Once the connection is established, you can send data over the TCP socket. This can be done using functions like `send()` or `write()` in most networking libraries. You pass the data you want to send as an argument to these functions.

5. **Receive Data (Optional)**: If your application expects a response from the server, you can also receive data over the TCP socket using functions like `recv()` or `read()`. This allows your application to process incoming data from the server.

6. **Close Connection**: After you've finished sending and receiving data, it's important to close the TCP connection to release system resources. You can do this using the `close()` function or method provided by your networking library.

Here's a simple example in Python using the `socket` library to send a TCP packet:

```python
import socket

# Destination server IP address and port
server_address = ('example.com', 80)

# Create a TCP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect to the server
sock.connect(server_address)

# Send data
data = b'Hello, server!'
sock.sendall(data)

# Close the connection
sock.close()
```

This code snippet opens a TCP socket, connects to a server at the specified IP address and port, sends a message, and then closes the connection. You can adapt this example to your specific use case and programming language.

Yes, that's correct! When you use `fetch` or make HTTP requests, you're interacting with the network at Layer 7, which is the application layer. This means you're sending data over HTTP or another application-level protocol, and the details of establishing connections and managing data transfer are abstracted away by the browser or HTTP client library you're using.

On the other hand, when you use sockets directly in your application code, you're working at a lower level, typically at Layer 4 (the transport layer) or even lower. With sockets, you have more control over the networking process, including the ability to send data over TCP or UDP connections, handle low-level protocols, and manage communication at a granular level.

Both approaches have their advantages and use cases:

1. **HTTP Requests (Layer 7)**:
   - Simplicity: HTTP requests are easy to use and understand, especially with high-level libraries like `fetch` in JavaScript.
   - Compatibility: HTTP is widely supported by web servers and APIs, making it a natural choice for web applications.
   - Abstraction: HTTP clients handle many networking details for you, such as connection management and error handling.

2. **Sockets (Layer 4)**:
   - Flexibility: Sockets provide low-level access to the network stack, allowing you to work with TCP, UDP, or other protocols directly.
   - Control: With sockets, you have full control over the networking process, including the ability to implement custom protocols or optimize performance.
   - Efficiency: Sockets can be more efficient for certain types of applications, especially those with specific networking requirements or performance constraints.

So, depending on your application's needs and requirements, you can choose between using HTTP requests for simplicity and compatibility or working with sockets for more control and flexibility.


# Gateway Load Balancer

The Gateway Load Balancer is a vital tool for efficiently managing and securing network traffic within AWS environments. Its primary function is to deploy, scale, and oversee a fleet of third-party network virtual appliances, such as firewalls, intrusion detection and prevention systems, and deep packet inspection tools. These appliances are crucial for analyzing and safeguarding network traffic, ensuring that all data passing through adheres to security protocols.

Operating at Layer 3 of the OSI model, the Gateway Load Balancer focuses on handling IP packets, making it an essential component for managing network traffic at a fundamental level. One notable feature is its support for the Geneve protocol, specifically on port 6081, which enhances its capabilities in handling diverse traffic types efficiently.

Users interact with the Gateway Load Balancer to analyze network traffic before it is redirected to designated target groups, which could consist of EC2 instances or specific IP addresses. This redirection ensures that all network traffic undergoes thorough scrutiny and filtering before reaching its intended destination, enhancing overall network security and efficiency.

# Elastic Load Balancer

Elastic Load Balancer (ELB) is a crucial component in distributed systems for managing incoming traffic efficiently across multiple instances. It supports sticky sessions, ensuring that a client's requests are consistently directed to the same instance on the backend. This functionality is available across various types of load balancers, including Classic Load Balancer, Application Load Balancer (ALB), and Network Load Balancer (NLB).

Sticky sessions rely on cookies for maintaining session affinity. ELB allows for the customization of the expiration date of these cookies, providing control over session duration. There are two primary types of cookies used for stickiness:

1. Application-based cookies: These are generated by the application itself, allowing for flexibility in session management. Users can specify the cookie name for each target group, enabling precise control over session routing.

2. Duration-based cookies: These are generated by the load balancer and have fixed names (`awsalb` for ALB and `awselb` for CLB). They offer a simpler approach to session stickiness, where the load balancer manages session persistence based on predefined durations.

Extending this functionality allows for greater customization and optimization of traffic distribution in distributed systems. Additionally, ELB's support for sticky sessions enhances user experience by ensuring continuity in session states, especially in applications that require persistent connections or stateful interactions. Furthermore, the ability to configure expiration dates for cookies provides flexibility in managing session lifetimes based on specific application requirements. Overall, ELB's sticky session feature enhances the reliability, scalability, and performance of applications hosted on cloud infrastructures.

# Elastic Load Balancer (Cross-Zone)

Summary: The Elastic Load Balancer (ELB) with Cross-Zone Load Balancing distributes incoming traffic evenly across all Availability Zones (AZs) associated with an application's backend instances, regardless of the number of instances in each AZ. This feature is automatically enabled for Application Load Balancers (ALBs) by default, allowing seamless distribution of traffic across multiple AZs without any additional configuration. Importantly, no data transfer charges apply when traffic flows between AZs, promoting cost efficiency for inter-AZ communication. Furthermore, network load is effectively managed, and no charges are incurred for intra-AZ data transfers.

Extension:
The implementation of Cross-Zone Load Balancing within the Elastic Load Balancer architecture significantly enhances the resilience and scalability of applications hosted across multiple Availability Zones within a region. By evenly distributing incoming traffic across all available zones, this feature helps prevent overloading of any single zone, thereby improving application performance and reliability. Moreover, with the default activation of Cross-Zone Load Balancing for Application Load Balancers, developers and administrators can streamline the deployment process, minimizing the need for manual intervention in load balancing configurations.

Furthermore, the elimination of data transfer charges for traffic between AZs represents a notable cost-saving advantage for organizations leveraging ELB for their applications. This cost predictability enables businesses to scale their infrastructure without facing unexpected expenses related to inter-AZ communication. Additionally, the efficient management of network load ensures optimal utilization of resources, contributing to a smoother and more responsive user experience.

In essence, the Elastic Load Balancer with Cross-Zone Load Balancing not only simplifies the deployment and management of applications across multiple AZs but also delivers cost efficiency and enhanced performance, making it a crucial component in architecting robust and scalable cloud-based solutions.

# Elastic Load Balancer (SSL)

The Elastic Load Balancer (SSL) facilitates encrypted traffic over networks, employing SSL/TLS protocols. SSL certificates, issued by Certificate Authorities (CA), ensure secure communication. These certificates can be linked to the load balancer, enabling in-flight encryption. Traffic flows from users to the load balancer via HTTPS, then to backend EC2 instances over HTTP. X.509 certificates are loaded onto the load balancer, supporting multiple domains and certificates. Server Name Indication (SNI) allows for the use of multiple certificates on one server, accommodating various websites. Both Application Load Balancers (ALB) and Network Load Balancers (NLB) support multiple certificates and SNI, unlike Classic Load Balancers, which only support one certificate.


# ELB Connection Draining

ELB Connection Draining is a feature designed to ensure seamless handling of requests during the process of de-registering instances from an Elastic Load Balancer (ELB). Here's a concise summary:

**Feature Naming:** ELB Connection Draining

**Deregistration Delay:** This is the time given to complete in-flight requests while an instance is being de-registered or is unhealthy.

**Time to Complete In-flight Requests:** Requests already in progress are allowed to finish while the instance is de-registering or marked as unhealthy.

**Halting New Requests:** ELB stops directing new requests to the EC2 instance being de-registered.

**User Connection Handling:** Users connected to the instance are provided a window to finish their existing connections/requests before the instance is de-registered.

**Draining State:** When an instance is in a draining state, the ELB doesn't route new requests to it.

**Customizable Draining Time:** The duration for draining can be set between 1 to 3600 seconds.


# Auto Scaling Group
Auto Scaling Group (ASG) is a feature in AWS that automates the scaling of EC2 instances based on demand. It can scale out by adding instances when demand increases and scale in by removing instances when demand decreases. Key components of ASG include setting minimum and maximum instance limits, linking to load balancers for distributing traffic, and automatically removing unhealthy instances. ASG is cost-efficient as you only pay for the resources used. Configurations include minimum capacity (minimum of 2 instances), desired capacity (initially set to 4 instances), and maximum capacity (up to 8 instances). Launch templates, which offer discounts, are used to specify configurations like AMI, instance type, user data, EBS volume, security groups, SSH key pair, IAM roles, network, subnets, and load balancer information. Scaling policies are set up using CloudWatch, where metrics such as average CPU usage or custom metrics trigger alarms that initiate scaling actions.

- **Auto Scaling:** Dynamically adjusts the number of EC2 instances based on demand.
- **Scaling Out/In:** Adds or removes instances to match demand fluctuations.
- **Minimum/Maximum Limits:** Sets boundaries for the number of instances in the group.
- **Integration with Load Balancer:** Links to distribute traffic efficiently.
- **Health Monitoring:** Automatically removes unhealthy instances.
- **Cost Efficiency:** Only pay for resources utilized.
- **Launch Templates:** Specify configurations including AMI, instance type, user data, etc.
- **IAM Roles:** Assigns roles for EC2 instances.
- **Scaling Policies:** Uses CloudWatch metrics to trigger scaling actions.
- **Alarm Integration:** Alerts on metrics like CPU usage to initiate scaling.


# Auto Scaling Groups - Scaling Policies

**Summary:**
Auto Scaling Groups (ASG) offer several scaling policies to efficiently manage resources based on demand. These include dynamic scaling with target tracking, simple step scaling triggered by CloudWatch alarms, scheduled scaling for anticipated usage patterns, and predictive scaling for continuous load forecasting.

**Description:**
Auto Scaling Groups (ASG) Scaling Policies provide a range of options for automatically adjusting computing resources based on demand. These policies ensure optimal performance and cost-efficiency by dynamically scaling capacity up or down as needed. Here are the key features:

**Dynamic Scaling / Target Tracking Policy:**
- Simplified setup process.
- Allows setting a target, such as maintaining an average ASG CPU utilization around a specific percentage (e.g., 40%).

**Simple / Step Scaling:**
- Responds to CloudWatch alarms triggered by specific conditions, like CPU utilization exceeding or falling below certain thresholds.
- Adds or removes units in steps, providing flexibility in resource allocation.
- For instance, adds 2 units when CPU > 70% and removes 1 unit when CPU < 30%.

**Scheduled Scaling:**
- Enables anticipating scaling needs based on known usage patterns.
- Allows setting specific times for scaling actions, like increasing minimum capacity to 10 at 5 PM on Fridays.

**Predictive Scaling:**
- Utilizes continuous load forecasting to schedule scaling actions in advance.
- Helps in efficiently managing resources by proactively adjusting capacity based on predicted demand fluctuations.

**Key Features:**
- Flexible scaling options: dynamic, step, scheduled, and predictive.
- Integration with CloudWatch for monitoring and triggering scaling actions.
- Simplified setup and configuration for different scaling scenarios.
- Improved resource utilization and cost optimization through automated scaling based on demand patterns.
    

# Scaling Policies: Good metrics
Scaling policies are crucial for efficiently managing resources in cloud environments. They allow systems to automatically adjust capacity based on various metrics to maintain performance and optimize costs. Here are some key features and metrics used in scaling policies:

1. **CPU Utilization**: This metric tracks the average CPU utilization across your instances. By monitoring CPU usage, scaling policies can dynamically adjust the number of instances to ensure that there's sufficient capacity to handle workload demands without over-provisioning resources.

2. **Request Count Per Target**: This metric focuses on the number of requests per EC2 instance, ensuring that the workload distribution remains balanced across instances. Keeping this metric stable helps prevent any single instance from becoming overloaded, maintaining overall system performance.

3. **Average Network In/Out**: Monitoring network traffic is essential for understanding communication patterns between instances and external services. Scaling policies can use average network data transfer rates to adjust capacity based on changing network demands, ensuring optimal performance and responsiveness.

4. **Custom Metrics from CloudWatch**: CloudWatch allows you to define custom metrics tailored to your specific application requirements. These could include application-specific performance indicators or business metrics relevant to your workload. By incorporating custom metrics into scaling policies, you can fine-tune capacity adjustments based on unique factors affecting your system.

In summary, scaling policies leverage various metrics such as CPU utilization, request counts, network activity, and custom metrics to dynamically adjust resource capacity in cloud environments. By monitoring and responding to these metrics, scaling policies help maintain performance, optimize resource utilization, and ensure cost efficiency.


# Scaling Cooldown

Summary:
Scaling Cooldown is a feature implemented in an auto-scaling group (ASG) that introduces a waiting period after a scaling event occurs. By default, this period lasts for 300 seconds. During the cooldown period, the ASG refrains from launching or terminating any additional instances. This pause is intended to allow time for system metrics to stabilize after the scaling activity.

Key Features:
1. Cooldown Period: After a scaling activity, such as adding or removing instances, a predefined cooldown period ensues. This period, set to 300 seconds by default, prevents further scaling actions within the ASG.
2. Stabilization of Metrics: The purpose of the cooldown period is to provide a window for system metrics to stabilize. This ensures that any subsequent scaling actions are based on reliable data, preventing rapid and unnecessary fluctuations in instance count.
3. Ready-to-Use AMI: To expedite configuration and minimize the cooldown period, users are advised to utilize ready-to-use Amazon Machine Images (AMIs). These pre-configured images enable quicker provisioning of instances, reducing the time needed to serve requests and consequently shortening the cooldown duration.

Highlight:
The Scaling Cooldown feature enhances the stability and efficiency of auto-scaling operations within an ASG by introducing a cooldown period after scaling events. By allowing metrics to stabilize and recommending the use of ready-to-use AMIs, it promotes faster response times and more effective resource management.
