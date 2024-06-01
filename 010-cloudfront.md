# Cloudfront & Global Accelerator

# CloudFront

CloudFront is a Content Delivery Network (CDN) service provided by AWS that enhances read performance by caching content at edge locations distributed globally. Here's an overview of its features:

- **CDN Service:** CloudFront acts as a global CDN, improving the delivery of web content to end users by caching it at edge locations closer to them.
- **Global Presence:** With 216 points of presence (PoPs) worldwide, CloudFront ensures low-latency content delivery to users regardless of their geographic location.
- **DDoS Protection:** CloudFront provides DDoS protection by leveraging AWS Shield and AWS Web Application Firewall (WAF), safeguarding your applications and content from malicious attacks.
- **S3 Bucket as Origin:** CloudFront can use an S3 bucket as its origin, with Origin Access Identity (OAI) ensuring secure access to the bucket content.
- **Ingress for Uploads:** CloudFront can also be used as an ingress point to upload files to S3, offering an efficient way to transfer data to AWS infrastructure.
- **Custom Origin:** Besides S3, CloudFront supports custom origins such as Application Load Balancers (ALB), EC2 instances, S3 websites, or any HTTP backend, providing flexibility in content delivery.
- **Caching:** Files are cached at edge locations based on a Time-to-Live (TTL) setting, typically for a day or as configured.

# ALB as Origin

When using an Application Load Balancer (ALB) as the origin for CloudFront, consider the following:

- **Accessing HTTP Backends:** CloudFront can access any HTTP backend, including applications hosted behind ALBs.
- **Public EC2 Instances:** To use ALB as an origin, EC2 instances behind the ALB must be publicly accessible, as CloudFront does not support private access.
- **Public Load Balancer:** The ALB must be configured to be publicly accessible to allow CloudFront to connect to it.
- **Edge Location Connectivity:** Ensure that the public IP addresses of CloudFront edge locations are allowed to connect to the ALB to facilitate content delivery.

# GEO Restrictions

CloudFront offers GEO restrictions to control access based on geographic location. Here's how it works:

- **Country Restriction:** You can restrict access based on the country of the viewer, determined using a third-party IP list.
- **Allow List:** Specify countries allowed to access content.
- **Block List:** Specify countries restricted from accessing content.
- **Copyright Protection:** GEO restrictions can be used to enforce copyright policies by limiting content access to authorized regions.

By leveraging CloudFront's capabilities such as global caching, integration with ALB, and GEO restrictions, you can optimize content delivery, enhance security, and enforce access controls for your applications and content delivery workflows.


# Price Classes

Price Classes in CloudFront allow you to control the geographic regions where your content is distributed and affect the pricing for content delivery. Here's an overview:

- **Edge Locations:** CloudFront's network spans globally, with edge locations strategically located around the world to improve content delivery performance.
- **Pricing Variations:** Pricing may vary depending on the region where content is delivered from.
- **Data Transfer Costs:** Typically, the more data transferred out from edge locations, the lower the unit cost.
- **Price Class Options:**
  - **All:** This includes all CloudFront edge locations globally, ensuring maximum coverage but potentially higher costs.
  - **100:** Covers edge locations in major regions such as the USA and Europe, offering a balance between coverage and cost.
  - **200:** Extends coverage to additional regions beyond the major ones, providing broader global reach at potentially higher costs.

You can optimize costs by selecting the appropriate price class based on your content delivery needs and target audience locations.

# Cache Invalidation

Cache Invalidation in CloudFront allows you to remove outdated or stale content from edge caches to ensure that users receive the most up-to-date content. Here are some key points:

- **TTL-based Invalidation:** Content is typically invalidated based on Time-to-Live (TTL) settings. When the TTL expires, CloudFront checks the origin for updated content.
- **Manual Invalidation:** You can initiate a manual cache invalidation to force CloudFront to fetch updated content from the origin.
- **File Path Invalidation:** Specify specific file paths or patterns (e.g., `/path/*`) to invalidate content associated with those paths.
- **Wildcard Invalidation:** Use wildcard characters (e.g., `*`) to invalidate all files in the distribution, forcing a refresh of the entire cache.

By leveraging cache invalidation, you can ensure that users receive the latest content from your CloudFront distribution, maintaining a seamless and up-to-date user experience.


# AWS Global Accelerator

AWS Global Accelerator is a networking service that allows you to improve the performance and availability of your applications by directing user traffic to the nearest AWS edge location. Here's an overview of its features:

- **Global Application Deployment:** Even if your application is deployed in only one AWS region, Global Accelerator leverages anycast IP addressing to direct users to the nearest edge location.
- **Anycast IP:** Anycast IP addressing allows multiple servers to share the same IP address, and users are routed to the nearest server. This optimization helps reduce latency and improve application performance.
- **AWS Internal Network:** Global Accelerator utilizes the AWS internal network to efficiently route traffic to your application, ensuring low-latency communication.
- **Compatible Services:** It works seamlessly with Elastic IP addresses, EC2 instances, Application Load Balancers (ALBs), Network Load Balancers (NLBs), and can handle both public and private endpoints.
- **Health Checks:** Global Accelerator performs health checks on your endpoints to ensure that traffic is only routed to healthy resources.
- **Automatic DDoS Protection:** It provides automatic DDoS protection through AWS Shield, safeguarding your application from malicious attacks.

# AWS Global Accelerator vs. CloudFront

While both AWS Global Accelerator and CloudFront aim to improve application performance and availability, they serve different use cases:

### CloudFront:
- **Content Caching:** CloudFront caches content at edge locations, serving cached content to users to reduce latency and improve scalability.
- **Edge-based Delivery:** Content is served from the edge locations closest to users, enhancing the delivery speed of static and dynamic content.

### Global Accelerator:
- **Direct-to-Origin Traffic:** Global Accelerator does not cache content; instead, it routes traffic directly to the backend servers without caching, improving performance for TCP or UDP-based applications.
- **Non-HTTP Use Cases:** It is well-suited for use cases such as gaming (UDP), IoT (MQTT), or voice over IP (VoIP), where real-time data transmission and low-latency communication are critical.

In summary, while CloudFront is optimized for content caching and edge-based delivery of HTTP content, Global Accelerator focuses on directing traffic to backend applications hosted in AWS regions, making it ideal for non-HTTP use cases and scenarios where direct-to-origin traffic routing is required.
