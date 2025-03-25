# EC2 - Elastic Cloud Compute
1. **What is Amazon EC2?**  
   - A cloud-based service that provides virtual servers on demand.  
   - Helps reduce hardware costs and speeds up application development and deployment.  

2. **Scalability**  
   - You can launch multiple virtual servers as needed.  
   - Scale up (add capacity) for heavy tasks or high traffic to handle compute-heavy tasks, such as monthly or yearly processes, or spikes in website traffic.   
   - Scale down (reduce capacity) when demand decreases.  

3. **EC2 Instance**  
   - A virtual server in the AWS Cloud.  
   - Runs applications without needing physical hardware.  

4. **Instance Type**  
   - Defines the hardware specifications (CPU(compute), memory, network, storage).  
   - Choose based on workload requirements.

![image](https://github.com/user-attachments/assets/89921cb0-335c-4738-b2ed-c079b300c6f1)

---

### Features of Amazon EC2  

1. **Instances** – Virtual servers that run applications in the cloud.  

2. **Amazon Machine Images (AMIs)** – Preconfigured templates with the operating system and necessary software for your server.  

3. **Instance Types** – Different hardware configurations for CPU, memory, storage, networking capacity, and graphics hardware based on workload needs.  

4. **Amazon EBS Volumes** – Persistent storage that retains data even after stopping an instance.  

5. **Instance Store Volumes** – Temporary storage that gets deleted when an instance stops, hibernates, or terminates.  

6. **Key Pairs** – Secure login system where AWS keeps the public key, and you store the private key.  

7. **Security Groups** – A virtual firewall that allows you to specify the protocols, ports, and source IP ranges that can reach your instances, and the destination IP ranges to which your instances can connect.  

8. **PCI DSS Compliance** – Supports the secure processing, storage, and transmission of credit card data.

---

# Introduction & Comprehensive Analysis of AWS EC2 Instance Types
When launching an EC2 instance, the instance type you select shapes the host computer's hardware, offering varying levels of compute, memory, storage, and networking. This choice is crucial for matching your application's needs, ensuring optimal performance and cost efficiency.

This detailed report expands on the key points, providing a thorough exploration of AWS EC2 instance types, their categories, and specifications, tailored for a DevOps intern beginning with AWS EC2. The analysis is based on provided documentation and additional resources, ensuring a complete understanding for practical application.

AWS EC2 (Elastic Compute Cloud) instance types define the hardware capabilities of virtual servers, including compute, memory, storage, and networking. Each instance type is grouped into families based on resource balance, catering to diverse application needs. The choice impacts performance, cost, and scalability, making it essential for DevOps professionals to understand these options.

EC2 instances are categorized to align with specific workloads, as outlined below, with examples and specifications drawn from the provided data.

### General Purpose Instances
General purpose instances offer a balanced mix of compute, memory, and networking, suitable for web servers, code repositories, and small databases. They include fixed performance instances (e.g., M5), burstable performance instances (e.g., T3), and flex instances (e.g., M7i-flex).

- **Fixed Performance (M5 Instances):**
  M5 instances, powered by Intel Xeon Platinum 8175 processors, are designed for demanding workloads.
  
<details>
   <summary>Specifications Include</summary>

  | Instance Type | vCPUs | Memory (GiB) | Network Performance |
  |---------------|-------|--------------|---------------------|
  | m5.large      | 2     | 8            | Up to 10 Gbps       |
  | m5.xlarge     | 4     | 16           | Up to 10 Gbps       |
  | m5.2xlarge    | 8     | 32           | Up to 10 Gbps       |
  Network specifications from the attachment show baseline bandwidth starting at 0.75 Gbps for m5.large, bursting to 10 Gbps, highlighting their capability for general applications.

</details>

- **Burstable Performance (T3 Instances):**
  T3 instances, part of the burstable family, provide a baseline CPU performance with burst capability, managed by CPU credits. They are cost-effective for applications with variable CPU needs, such as development environments.

<details>
   <summary>Specifications Include</summary>

  | Instance Type | vCPUs | Memory (GiB) | Baseline CPU Performance |
  |---------------|-------|--------------|--------------------------|
  | t3.medium     | 2     | 4            | 20%                      |
  | t3.large      | 2     | 8            | 30%                      |
  The attachment confirms T3 uses Intel Skylake P-8175, with no accelerators, focusing on flexibility.

</details>

- **Flex Instances (M7i-flex, C7i-flex):**
  Flex instances, like M7i-flex, offer a cost-effective balance, with reliable CPU at 40% baseline, bursting to 100% for 95% of a 24-hour period. They are ideal for general-purpose workloads, with specifications similar to M5 but optimized for cost.

### Compute Optimized Instances
Compute optimized instances, such as C5, are tailored for compute-intensive tasks like batch processing and media transcoding, leveraging high-performance processors.

<details>
   <summary>Specifications Include</summary>

  | Instance Type | vCPUs | Memory (GiB) | Network Performance |
  |---------------|-------|--------------|---------------------|
  | c5.large      | 2     | 4            | Up to 10 Gbps       |
  | c5.xlarge     | 4     | 8            | Up to 10 Gbps       |
  | c5.2xlarge    | 8     | 16           | Up to 10 Gbps       |
  Powered by Intel Xeon Platinum 8124M, these instances ensure consistent performance, with no burstable options, as confirmed by the attachment.

</details>

### Memory Optimized Instances
Memory optimized instances, like R5, are designed for in-memory processing, suitable for databases and real-time analytics.

<details>
   <summary>Specifications Include</summary>

  | Instance Type | vCPUs | Memory (GiB) | Network Performance |
  |---------------|-------|--------------|---------------------|
  | r5.large      | 2     | 16           | Up to 10 Gbps       |
  | r5.xlarge     | 4     | 32           | Up to 10 Gbps       |
  | r5.2xlarge    | 8     | 64           | Up to 10 Gbps       |
  Using Intel Xeon Platinum 8175, R5 instances offer high memory capacity, with no accelerators, focusing on memory-intensive tasks.
  
</details>



### Storage Optimized Instances
Storage optimized instances, such as I3, are for workloads requiring high I/O, like NoSQL databases.

<details>
   <summary>Specifications Include</summary>

  | Instance Type | vCPUs | Memory (GiB) | Storage (GB) | Network Performance |
  |---------------|-------|--------------|--------------|---------------------|
  | i3.large      | 2     | 15.25        | 475 NVMe SSD | Up to 10 Gbps       |
  | i3.xlarge     | 4     | 30.5         | 950 NVMe SSD | Up to 10 Gbps       |
  These instances provide NVMe SSD storage, optimized for low-latency, high-throughput operations.
  
</details>



### Accelerated Computing Instances
Accelerated computing instances, like P3, use GPUs for tasks like machine learning.

<details>
   <summary>Specifications Include</summary>

  | Instance Type | vCPUs | Memory (GiB) | GPUs | GPU Memory (GiB) | Network Performance |
  |---------------|-------|--------------|------|------------------|---------------------|
  | p3.2xlarge    | 8     | 61           | 1    | 16               | 10 Gbps             |
  | p3.8xlarge    | 32    | 244          | 4    | 64               | 25 Gbps             |
  Powered by NVIDIA Tesla V100, these are ideal for high-performance computing, with significant GPU memory.
  
</details>

#### Additional Features and Considerations
- **Burstable Performance Management:** Governed by CPU credits, earned below baseline and spent when bursting, as detailed in the provided text, making T3 instances cost-effective for variable workloads.
- **Flex Instances:** M7i-flex and C7i-flex offer a balance, with bursting capabilities up to 100% CPU for 95% of 24 hours, enhancing cost efficiency for general purposes.
- **Nitro System:** The attachment highlights the AWS Nitro System, enhancing performance and security, with versions like Nitro v5 supporting up to 200 Gbps network, relevant for advanced instances.

#### Regional Availability and Practical Implications
The provided text lists instance types available in regions like US East (N. Virginia), with variations by region, affecting availability. For instance, M5 is widely available, but checking regional support is crucial for deployment planning.

---

# Key Pair
Key pairs are used for secure access to EC2 instances. They consist of a public key (stored by AWS) and a private key (downloaded by the user). For Linux, use SSH with the private key; for Windows, use RDP (Remote Desktop Protocol) to decrypt the administrator password.
  - **Theoretical Insight:** Key pairs enhance security by ensuring only authorized users can access instances.
  - **Practical Example:** Create a key pair in the AWS Console, download the .pem file, and use it to SSH into an EC2 instance: `ssh -i mykey.pem ec2-user@public-ip`.

---

# Pricing Models
- **Pricing Models:** AWS offers several pricing models for EC2:
  - **On-Demand Instances:** Pay by the hour or second, no long-term commitment, ideal for unpredictable workloads.
  - **Reserved Instances:** Commit for 1 or 3 years for discounts, suitable for steady-state applications.
  - **Spot Instances:** Bid on spare capacity at reduced prices, best for flexible, fault-tolerant applications.
  - **Dedicated Hosts:** Physical servers for licensing or compliance, higher cost.
  - **Savings Plans:** Commit to consistent usage for lower prices, flexible across services.
  - **Theoretical Insight:** Pricing models optimize costs based on workload predictability and duration.
  - **Practical Example:** Compare costs using the [AWS Pricing Calculator](https://calculator.aws/), selecting on-demand for a short-term project versus reserved for a long-term database.

---

# Launch Template
#### What is a Launch Template?
A Launch Template in AWS is a resource that allows users to save and reuse configurations for launching EC2 (Elastic Compute Cloud) instances. It acts as a blueprint, containing all necessary parameters to launch an instance, such as the Amazon Machine Image (AMI), the size of the virtual machine (instance type), key pair, security groups, and more. This ensures consistency and efficiency, particularly in automated and scaled environments, which is vital for DevOps practices.

#### Key Components and Features
When creating a Launch Template, users can specify a wide range of parameters, ensuring flexibility for various use cases. The main components include:

- **AMI ID:** The identifier for the Amazon Machine Image, which defines the operating system and pre-installed software (e.g., Amazon Linux, Ubuntu, Windows Server).
- **Instance Type:** Determines the hardware capabilities, such as vCPUs, memory, and network performance (e.g., t2.micro for small tasks, m5.large for general purpose).
- **Key Pair:** Used for secure SSH access to the instance, ensuring only authorized users can connect.
- **Security Groups:** Define firewall rules to control inbound and outbound traffic, enhancing security.
- **Storage:** Configuration for Elastic Block Store (EBS) volumes, including size, type (e.g., gp3, io2), and whether volumes are deleted on instance termination.
- **Network Interfaces:** Specify subnet, private IP addresses, and whether to associate a public IP, crucial for networking setups.
- **IAM Role:** An Identity and Access Management role that grants the instance permissions to access other AWS services, improving security by avoiding embedded credentials.
- **User Data:** Scripts or commands executed on instance startup, often used for bootstrapping applications or installing software.
- **Tags:** Key-value pairs for organizing and managing resources, aiding in cost allocation and resource tracking.

Once saved, you can use this template to launch instances directly or with Auto Scaling, which automatically adjusts the number of instances based on demand. An unexpected detail is that Launch Templates support versioning, meaning you can save different versions for testing new settings without losing the old ones, which is great for updates.

#### Use Cases in DevOps
For DevOps professionals, Launch Templates are invaluable for several reasons, aligning with modern cloud practices:

1. **Consistency Across Environments:** By using the same Launch Template (or versions) across development, staging, and production, you ensure identical instance configurations, reducing the "it works on my machine" problem. For example, a web server setup can be standardized across all environments, ensuring predictable behavior.

2. **Automation in CI/CD Pipelines:** Integrate Launch Templates with CI/CD tools like Jenkins or GitHub Actions. When deploying a new application version, update the Launch Template with a new AMI ID, then trigger an Auto Scaling group update, enabling seamless deployments without manual intervention.

3. **Scalability and Elasticity:** Use with Auto Scaling to dynamically adjust instance counts based on metrics like CPU utilization or network traffic, ensuring the application scales efficiently while maintaining configuration consistency.

4. **Cost Optimization:** Specify Spot instances in the Launch Template for cost savings on fault-tolerant workloads, or use capacity reservations for predictable capacity, balancing cost and performance.

5. **Security and Compliance:** Enforce security best practices by including IAM roles for least privilege access and security groups for network control, ensuring compliance across all instances.

An advanced use case is sharing Launch Templates across AWS accounts using AWS Resource Access Manager (RAM), enabling organizations with multiple accounts to standardize configurations, though this requires additional setup.

#### Practical Implementation
To create and use a Launch Template, follow these steps:

1. **Creation:**
   - Navigate to the EC2 console, select "Launch Templates," and click "Create launch template."
   - Fill in details: name, description, AMI ID, instance type, key pair, security groups, etc.
   - Optionally, specify user data (e.g., a script to install Apache: `#!/bin/bash\nyum update -y\nyum install httpd -y\nservice httpd start`), tags, and advanced settings.
   - Save the template, which creates version 1.

2. **Usage:**
   - Launch an instance directly from the template by selecting "Launch instance from template" in the EC2 console.
   - Or, create an Auto Scaling group, specifying the Launch Template and desired capacity.
   - Use the AWS CLI for automation: `aws ec2 create-launch-template --launch-template-name MyTemplate --version-description "Initial version" --launch-template-data file://template.json`, where `template.json` contains the configuration.

For example, a template for a web server might include:
- AMI: Amazon Linux 2
- Instance type: t3.medium
- Key pair: my-key-pair
- Security group: allow HTTP/HTTPS
- User data: install and start Apache

This can then be used to launch multiple instances consistently.

#### Best Practices
To manage Launch Templates effectively, consider:
- **Version Control:** Create new versions for significant changes, document each version’s purpose, and use versioning for rollback strategies.
- **Parameterization:** Use AWS Systems Manager Parameter Store to manage dynamic values (e.g., database connection strings), enhancing flexibility and security.
- **Tagging:** Apply consistent tags for cost allocation, resource tracking, and compliance.
- **Monitoring and Auditing:** Regularly review and update templates to align with current requirements, using AWS CloudTrail for audit logs.

#### Tables for Clarity
Below is a table comparing Launch Templates and Launch Configurations:

| Feature                  | Launch Template          | Launch Configuration      |
|--------------------------|--------------------------|---------------------------|
| Versioning               | Supported, multiple versions | Not supported             |
| Multiple Network Interfaces | Yes                     | No                        |
| On-Demand and Spot Mix   | Yes                     | No                        |
| Advanced Storage Options | Yes                     | Limited                   |
| Integration with Parameter Store | Yes               | No                        |
| Preferred by AWS         | Yes, for new applications | Legacy, limited updates   |

And a table of common Launch Template parameters:

| Parameter           | Description                                      | Example                     |
|---------------------|--------------------------------------------------|-----------------------------|
| AMI ID              | Image for the instance                           | ami-0c55b159cbfafe1f0       |
| Instance Type       | Hardware configuration                           | t3.medium                   |
| Key Pair            | For SSH access                                   | my-key-pair                 |
| Security Groups     | Network traffic rules                            | sg-1234567890abcdef0        |
| EBS Volumes         | Storage configuration                            | 20 GiB, gp3, encrypted      |
| User Data           | Startup scripts                                  | Install Apache, start service |
| IAM Role            | Permissions for AWS services                     | EC2-S3-ReadOnly             |

---

# Amazon Elastic Block Store (EBS)
EBS is a block storage service designed for use with EC2 instances, offering persistent storage volumes. It is analogous to an external hard drive that can be attached to a single EC2 instance, providing high performance and durability.
- Think of this as an external hard drive for your EC2 instance. It stores data persistently, meaning it stays even if you stop or terminate the instance. It's great for databases or file systems where you need to keep data long-term.

- **Key Characteristics:**
  - **Persistence:** Data on EBS volumes persists independently of the instance's lifecycle, remaining available even after instance stops or terminations.
  - **Attachment:** Typically attached to one EC2 instance at a time, though multi-attach is supported for specific volume types like io2 Block Express.
  - **Performance:** Offers various volume types, such as General Purpose SSD (gp3, gp2), Provisioned IOPS SSD (io1, io2), Throughput Optimized HDD (st1), and Cold HDD (sc1), each optimized for different workloads. For example, gp3 volumes balance price and performance, suitable for boot volumes and development environments.
  - **Durability:** Automatically replicated within its Availability Zone, offering 99.999% durability.
  - **Size and Scalability:** Volumes can range from 1 GB to 64 TB, with manual resizing required for scaling.

- **Use Cases:** Ideal for databases (both relational and non-relational), file systems, and applications requiring persistent storage, such as software testing and development environments. For instance, a MySQL database would benefit from EBS for its low-latency, high-IOPS needs.

- **Practical Example:** Create an EBS volume, attach it to an EC2 instance, format it with a file system (e.g., ext4), and mount it as a persistent drive for storing application data.

---

# Amazon Elastic File System (EFS)
EFS is a managed file storage service that provides a scalable, elastic file system for use with EC2 instances, accessible across multiple instances simultaneously. It is akin to a network drive, supporting POSIX-compliant file access for Linux-based workloads.
- Imagine a network drive that multiple EC2 instances can access at once. It's perfect for sharing files, like in content management systems, and scales automatically as you add data.

- **Key Characteristics:**
  - **Persistence:** Data is persistent, with automatic scaling as files are added or removed, without manual provisioning.
  - **Attachment:** Can be mounted on multiple EC2 instances across different Availability Zones, supporting shared access.
  - **Performance:** Offers high throughput and low latency, with options for Standard and Infrequent Access tiers for cost optimization. It automatically scales to petabytes of storage and gigabytes per second of throughput.
  - **Durability and Availability:** Designed for 99.999999999% (11 9s) durability and up to 99.99% (4 9s) availability, with data replicated across multiple Availability Zones in Regional file systems.
  - **Compatibility:** Primarily for Linux instances, using NFSv4 protocol, with integration for AWS compute services like ECS, EKS, and Lambda.

- **Use Cases:** Suitable for enterprise-wide file servers, backup systems, Big Data clusters, Content Distribution Networks (CDNs), and applications requiring shared file access, such as home directories or media sharing.

- **Practical Example:** Create an EFS file system, mount it on multiple EC2 instances, and use it to share logs or media files, ensuring all instances have access to the same data.

---

# Instance Store
Instance Store provides temporary block-level storage physically attached to the host computer of an EC2 instance. It is ephemeral, meaning data is lost if the instance is stopped, terminated, or if the underlying host fails, making it suitable for non-critical, recreatable data.
- This is like the RAM of your computer—fast but temporary. Data here is lost if the instance stops or terminates, so it's best for caches or buffers that can be recreated.


- **Key Characteristics:**
  - **Persistence:** Temporary; data is not persistent through instance stops, terminations, or hardware failures.
  - **Attachment:** Attached to a single EC2 instance, with performance tied to local disk access.
  - **Performance:** Offers high I/O performance due to direct attachment, with sub-millisecond latencies, particularly for SSD-based instance stores.
  - **Types:** Can be HDD, SSD, or NVMe SSD, depending on the instance type, with varying capacities (e.g., up to 50 GB for certain instance types).

- **Use Cases:** Best for caches, buffers, and temporary computation data that can be regenerated, such as session stores or scratch space for processing.

- **Practical Example:** Launch an EC2 instance with Instance Store, use it for temporary processing like caching frequently accessed data, and note that data is lost upon instance termination.

---

#### Comparative Analysis
To highlight the differences, here's a table comparing EBS, EFS, and Instance Store:

| Feature                  | EBS (Elastic Block Store)          | EFS (Elastic File System)          | Instance Store                   |
|--------------------------|------------------------------------|------------------------------------|----------------------------------|
| **Type**                 | Block storage                     | File storage                      | Block storage                   |
| **Persistence**          | Persistent                        | Persistent                        | Temporary (ephemeral)           |
| **Multi-Instance Access** | Single instance (multi-attach possible for io2 Block Express) | Multiple instances across AZs | Single instance                 |
| **Scalability**          | Manual resizing, up to 64 TB      | Automatic, unlimited scale        | Fixed by instance type          |
| **Performance**          | Customizable IOPS, low latency    | High throughput, less latency focus | High I/O, sub-millisecond latency |
| **Use Case**             | Databases, boot volumes           | Shared data, content management   | Caches, buffers                 |
| **Cost**                 | Cheaper per GB, provisioning needed | More expensive, dynamic scaling   | Included in instance pricing    |
| **Durability**           | 99.999% within AZ                 | 99.999999999% (11 9s)            | Not durable, data loss possible |

This comparison underscores EBS for persistent, single-instance needs, EFS for shared, scalable file access, and Instance Store for temporary, high-performance storage.

---

# Snapshots and Backup Options

### EBS Snapshots
EBS snapshots are point-in-time copies of EBS volumes, stored in Amazon S3 for durability. They are incremental, meaning only changes since the last snapshot are saved, enhancing cost-efficiency and speed.
- These are like taking a photo of your external hard drive (EBS volume) at a specific moment. You can use it to back up data, create new volumes, or restore if something goes wrong. They're stored in S3 and only save changes after the first snapshot, saving space.

- **Key Features:**
  - **Creation:** Can be created manually via the AWS Console, CLI, or SDK, or automated using AWS Data Lifecycle Manager.
  - **Storage:** Stored in S3, with encryption options using AWS KMS for security.
  - **Use Cases:** Backup for disaster recovery, creating new volumes, migrating data across regions or accounts.
  - **Performance:** Supports lazy loading, allowing immediate access to volumes created from snapshots, with optional Fast Snapshot Restore for low-latency access.

- **Practical Example:** Take an EBS snapshot of a database volume, then restore it to a new volume in another region for disaster recovery, ensuring data integrity.

### EC2 AMIs and Snapshots
While EC2 instances themselves do not have "snapshots," creating an Amazon Machine Image (AMI) from an EC2 instance captures its configuration, including the operating system and any attached EBS volumes. This process involves creating snapshots of the EBS volumes, which are then referenced by the AMI.
- **EC2 Snapshots (via AMIs):** When you create an AMI from an EC2 instance, it's like saving the entire setup, including the operating system and any EBS volumes. This lets you launch new instances with the same configuration, useful for scaling or recovery. An unexpected detail is that AMIs include snapshots of EBS volumes, linking back to EBS snapshot functionality.

- **Key Features:**
  - **Creation:** From the EC2 Console, select "Create Image" to generate an AMI, which includes snapshots of EBS volumes.
  - **Storage:** Snapshots are stored in S3, with the AMI serving as a template for launching new instances.
  - **Use Cases:** Launching identical instances for scaling, disaster recovery, or consistent deployments across environments.
  - **Performance:** AMIs enable faster launches compared to Instance Store-backed instances, with persistent storage via EBS.

- **Practical Example:** Create an AMI from a running web server instance, then use it to launch multiple instances in an Auto Scaling group, ensuring all have the same setup, including pre-installed software.

#### Best Practices and Considerations
- **Choosing Storage:** For persistent data needing single-instance access, use EBS. For shared file access, EFS is ideal. For temporary, high-performance needs, Instance Store is suitable, but ensure data can be recreated.
- **Snapshot Management:** Regularly schedule EBS snapshots for backups, using AWS Backup for automation. For EC2, create AMIs for critical instances to ensure rapid recovery.
- **Cost Optimization:** EBS volumes can be resized or switched to cost-effective types like gp3. EFS offers Infrequent Access tiers for lower costs on rarely accessed data. Instance Store is included in instance pricing, reducing additional costs for temporary data.

---
