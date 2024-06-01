# Containers

# Containers Introduction

**Container Section**

- **Use Case**: 
  - Microservice architecture is a common use case.
  
- **Running Docker Images**:
  - Docker agents need to be running.
  - Multiple Docker containers of the same application can run simultaneously.
  
- **Image Storage**:
  - Images can be stored in various repositories like DockerHub, Amazon ECR, or private repositories.
  - Both public and private repositories are available.

- **Differences from VM**:
  - Docker operates differently from traditional virtual machines (VMs).
  - Docker utilizes a Docker daemon, whereas VMs use a hypervisor.

- **AWS Services**:
  - Amazon ECS (Elastic Container Service) is an AWS service for managing containerized applications.
  - Amazon EKS (Elastic Kubernetes Service) is a managed Kubernetes service.
  - AWS Fargate is a serverless compute engine for containers.
  - Amazon ECR (Elastic Container Registry) is a fully managed Docker container registry.

In summary, containers are widely used for microservice architecture, with Docker being a popular choice. AWS offers several services for container management, including ECS, EKS, Fargate, and ECR, providing flexibility and scalability for containerized applications.

Here's the breakdown of Amazon ECS:

**Amazon ECS (Elastic Container Service)**

- **Launch Types**:
  - **EC2 Launch Type**:
    - Requires provisioning and maintaining infrastructure (EC2 instances).
    - Cluster consists of multiple EC2 instances, each running an ECS agent.
    - ECS tasks start and stop Docker containers on these instances.
  - **Fargate Launch Type**:
    - Serverless approach; no need to provision infrastructure.
    - Task definitions are created, and AWS runs the tasks based on CPU/RAM requirements.
    - Scaling is achieved by increasing the number of tasks.

- **IAM Roles for ECS**:
  - **EC2 Instance Profile (EC2 Launch Type)**:
    - Used by the ECS agent running on EC2 instances.
    - Allows ECS agent to make API calls to ECS service, send container logs to CloudWatch, and pull Docker images from ECR.
    - Can reference sensitive data in Secrets Manager or SSM Parameter Store.
  - **ECS Task Role**:
    - Allows each ECS task to have a specific role.
    - Different roles can be used for different ECS services.
    - Task role permissions are defined in task definitions.

- **Load Balancer Integrations**:
  - **ALB (Application Load Balancer)**:
    - Supported and works for most use cases.
  - **NLB (Network Load Balancer)**:
    - Recommended for high throughput/high-performance use cases or when paired with AWS PrivateLink.
  - **Classic Load Balancer**:
    - Not supported.

- **Data Volumes (EFS)**:
  - EFS can be mounted onto ECS tasks.
  - Works for both EC2 and Fargate launch types.
  - Tasks running in any Availability Zone share the same data in EFS.
  - Fargate + EFS combination provides serverless shared storage.
  - Use cases include persistent multi-AZ shared storage for containers.
  - Note: S3 cannot be mounted as a file system.

Amazon ECS offers flexibility in managing containers, supporting both EC2 and Fargate launch types, with various integrations for load balancing and data volumes. Additionally, IAM roles provide fine-grained access control for ECS tasks.

Here's the breakdown of ECS Cluster and ECS Service:

**ECS Cluster:**
- Supports both Fargate and EC2 (Auto Scaling Groups).
- For EC2, desired capacity for Auto Scaling Groups can be set to 1, ensuring a single instance is always running and registered in the cluster.

**ECS Service:**
- Before creating a service, a task definition needs to be created.
- Task definitions can choose between Fargate and EC2 instance modes, allowing tasks to be started on Fargate or EC2 instances.
- An IAM role needs to be assigned to a task if API calls to other AWS services are required.
- Container settings include port mapping, environment variables, etc.
- Launching a task definition as a service involves specifying its desired count, placement constraints, and task placement strategies.

Sure, let's break down the key components in Amazon ECS (Elastic Container Service) and their differences:

1. **Task**: 
   - A task is the smallest unit of work in ECS.
   - It represents a set of containerized applications that should be run together.
   - A task definition specifies which Docker images to use, how many containers are in the task, and how they interact.
   - Tasks can be launched as part of a service or manually through the ECS console or API.
   - A task can consist of one or more containers, which are treated as a single logical unit.

2. **Service**:
   - A service in ECS manages and maintains a specified number of instances of a task definition.
   - It ensures that the desired number of tasks (instances) are running and restarts them if they fail or stop.
   - Services allow for load balancing and scaling of tasks across multiple EC2 instances or Fargate containers.
   - They provide a way to scale containers horizontally and distribute traffic across them.
   - Services can be configured to use a variety of load balancers for distributing traffic.

3. **Cluster**:
   - An ECS cluster is a logical grouping of container instances or Fargate tasks.
   - It acts as the foundation for ECS, providing the infrastructure where tasks are scheduled and run.
   - Clusters can contain EC2 instances (which run the ECS agent) or Fargate tasks.
   - Multiple services can be deployed within a cluster, each with its own set of tasks.

4. **Container Instance**:
   - A container instance is an EC2 instance (in EC2 launch type) or an isolated compute environment (in Fargate launch type) that runs containers.
   - It's part of an ECS cluster and can run multiple tasks concurrently.
   - Container instances must have the ECS agent running to register with the ECS cluster and receive task definitions.

5. **Task Definition**:
   - A task definition is a blueprint for a task.
   - It defines which Docker images to use, how many containers are in the task, and how they interact.
   - Task definitions also specify resource requirements, container definitions (like CPU, memory, networking), logging configuration, etc.
   - Task definitions are versioned, allowing multiple revisions to be stored and used.

In summary, tasks represent individual workloads, services manage and maintain a specified number of tasks, clusters provide the infrastructure for running tasks, container instances execute tasks, and task definitions define the configuration for tasks. Each component plays a critical role in orchestrating containerized applications within ECS.

# ECS Auto Scaling
ECS Auto Scaling provides dynamic scaling capabilities to ensure that your ECS tasks can handle varying levels of workload demand efficiently. Here's a breakdown of ECS Auto Scaling and related concepts:

1. **Auto Scaling Policies**:
   - Automatically adjusts the number of ECS tasks based on specified criteria like CPU utilization, memory usage, or custom CloudWatch metrics.
   - Scaling policies can be configured using target tracking, step scaling, or scheduled scaling.

2. **Target Tracking Scaling**:
   - Scales ECS tasks based on a target value for a specific CloudWatch metric, such as CPU utilization or request count.
   - Automatically adjusts the number of tasks to maintain the target value.

3. **Step Scaling**:
   - Scales ECS tasks based on CloudWatch alarms and scaling adjustment steps.
   - Allows more granular control over scaling actions by defining specific thresholds and actions.

4. **Scheduled Scaling**:
   - Allows you to schedule changes to the number of ECS tasks at specific times.
   - Useful for predictable changes in workload demand, such as during peak hours or scheduled maintenance windows.

5. **Fargate Auto Scaling**:
   - Simplifies auto-scaling for Fargate tasks by automatically provisioning and scaling the underlying infrastructure based on resource requirements.
   - Provides a serverless experience without the need to manage EC2 instances.

6. **EC2 Instance Auto Scaling**:
   - Scales the EC2 instances within the ECS cluster based on criteria like CPU utilization or custom CloudWatch metrics.
   - Managed by Auto Scaling Groups (ASGs), which automatically add or remove EC2 instances to meet the desired capacity.

7. **ECS Cluster Capacity Providers**:
   - Automatically provisions and scales EC2 instances within ECS clusters to ensure sufficient capacity for running tasks.
   - Paired with Auto Scaling Groups to add or remove EC2 instances dynamically based on workload demand.

8. **Event-Based Task Invocation**:
   - ECS tasks can be invoked based on events from other AWS services, such as S3 uploads or messages in SQS queues.
   - EventBridge (formerly CloudWatch Events) can trigger ECS tasks based on predefined rules, allowing for event-driven scaling and task execution.

9. **Intercepting Stopped Tasks**:
   - EventBridge can be used to intercept events when ECS tasks are stopped.
   - This can trigger actions like sending notifications or executing cleanup tasks, providing visibility and control over task lifecycle events.

By leveraging ECS Auto Scaling and related features, you can ensure that your ECS tasks can dynamically adapt to changing workload conditions, improving efficiency and reliability in your containerized environment.

# Amazon ECR
Amazon Elastic Container Registry (ECR) is a fully managed Docker container registry service provided by AWS. Here's an overview of its key features and functionalities:

1. **Private and Public Registries**:
   - ECR supports both private and public container registries.
   - Private registries are secured and accessible only to authorized users within your AWS account.
   - Public registries, such as gallery.ecr.aws, provide a curated collection of publicly available container images.

2. **Integration with IAM**:
   - IAM roles are used to control access to ECR resources.
   - Policies can be defined to grant or restrict permissions for users and services to push, pull, or manage container images.

3. **Secure Storage**:
   - Container images are securely stored within ECR repositories.
   - Images are durably stored in Amazon S3, ensuring high availability and durability.

4. **Image Lifecycle Management**:
   - Supports versioning of container images, allowing you to maintain multiple versions of the same image.
   - Images can be tagged with labels for organization and identification purposes.

5. **Image Scanning**:
   - ECR provides built-in image vulnerability scanning capabilities.
   - Automatically scans container images for known security vulnerabilities and issues.

6. **Container Image Push and Pull**:
   - Docker CLI and other container management tools can be used to push container images to ECR repositories.
   - Authorized users and services can pull images from ECR repositories to deploy containers in ECS, EKS, or other container orchestration platforms.

7. **Image Replication**:
   - Supports cross-region replication of container images to improve availability and reduce latency for distributed applications.

8. **Integration with AWS Services**:
   - Seamlessly integrates with AWS services like ECS and EKS, allowing you to deploy containerized applications using images stored in ECR.
   - Provides native support for AWS Identity and Access Management (IAM) for fine-grained access control.

9. **Private Network Access**:
   - Supports VPC endpoints for secure and private access to ECR within your AWS Virtual Private Cloud (VPC).

Amazon ECR simplifies the process of storing, managing, and deploying container images, providing a secure and reliable registry solution for your containerized applications.

# EKS
Amazon Elastic Kubernetes Service (EKS) is a managed Kubernetes service offered by AWS. Here's an overview of its key features and functionalities:

1. **Kubernetes Management**:
   - Provides a fully managed Kubernetes control plane, allowing you to deploy, scale, and manage containerized applications using Kubernetes.
   - Offers compatibility with the Kubernetes API, enabling seamless integration with existing Kubernetes tools and workflows.

2. **Launch Types**:
   - Supports both Fargate and EC2 launch types.
   - Fargate launch type eliminates the need to manage underlying EC2 instances, providing a serverless Kubernetes experience.
   - EC2 launch type allows you to manage and customize the underlying EC2 instances that host your Kubernetes nodes.

3. **Node Types**:
   - **Managed Node Groups**: EKS automatically creates and manages EC2 instances (nodes) for you. These nodes are part of Auto Scaling Groups (ASGs) managed by EKS.
   - **Self-managed Nodes**: You can create and manage your own EC2 instances and register them with the EKS cluster. These nodes can be provisioned using prebuilt Amazon EKS-optimized Amazon Machine Images (AMIs) and can be part of ASGs managed by you.

4. **Networking**:
   - Integrates with Amazon VPC to provide network isolation for Kubernetes clusters.
   - Supports Kubernetes Network Policies for fine-grained network access control between pods.

5. **Integration with AWS Services**:
   - Seamlessly integrates with other AWS services such as ECR, IAM, CloudWatch, and CloudFormation.
   - Enables you to leverage AWS security features and services for authentication, authorization, monitoring, and logging.

6. **Load Balancer Integration**:
   - Automatically provisions AWS Elastic Load Balancers (ELBs) or Network Load Balancers (NLBs) to expose Kubernetes services to external traffic.
   - Supports integration with AWS Application Load Balancers (ALBs) through Ingress resources.

7. **Scaling and High Availability**:
   - Provides built-in support for horizontal scaling and high availability of Kubernetes applications.
   - Utilizes Auto Scaling Groups (ASGs) to automatically scale EC2 instances based on CPU/memory utilization or other metrics.

8. **Attach Data Volumes**:
   - Supports attaching persistent storage volumes to Kubernetes pods using StorageClasses and Container Storage Interface (CSI) compliant drivers.
   - Integrates with various AWS storage services such as Amazon EBS, Amazon EFS, FSx for Lustre, and FSx for NetApp ONTAP.

Amazon EKS simplifies the process of deploying and managing Kubernetes clusters on AWS, providing a scalable, reliable, and secure platform for running containerized applications.

# AWS App Runner Service

**AWS App Runner** is a fully managed service designed to simplify the deployment of web applications and APIs at scale. Here are its key features and capabilities:

1. **Fully Managed Service**:
   - Requires no prior infrastructure experience, making it accessible to developers of all levels.
   - Handles the entire deployment process, from provisioning resources to managing scaling and availability.

2. **Source Code or Container Image Deployment**:
   - Supports deploying applications directly from your source code or container images.
   - Offers flexibility in how you package and deploy your applications.

3. **Automatic Build and Deployment**:
   - Automates the build and deployment process, reducing the need for manual intervention.
   - Allows developers to focus on coding while App Runner takes care of the deployment pipeline.

4. **Automatic Scaling**:
   - Scales resources automatically based on application traffic and load.
   - Ensures that your applications are highly available and can handle varying levels of demand.

5. **Load Balancer and Encryption**:
   - Provides built-in load balancing capabilities to distribute traffic across multiple instances of your application.
   - Ensures data security through encryption mechanisms to protect sensitive information in transit and at rest.

6. **VPC Access Support**:
   - Allows applications deployed on App Runner to securely access resources within your Virtual Private Cloud (VPC).
   - Provides network isolation and control over inbound and outbound traffic flow.

7. **Integration with AWS Services**:
   - Enables seamless integration with other AWS services such as databases, caching solutions, and message queues.
   - Allows you to build complex architectures by connecting your application to various AWS resources.

8. **Use Cases**:
   - Well-suited for deploying various types of web applications, APIs, and microservices.
   - Ideal for scenarios requiring rapid production deployments, where simplicity, scalability, and reliability are paramount.

Overall, AWS App Runner simplifies the process of deploying and managing web applications and APIs, allowing developers to focus on building great software without worrying about infrastructure management.
