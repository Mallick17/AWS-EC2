# AWS-EC2
## 1. AMI, Instance Types, and Key Pairs

### 1.1 Amazon Machine Images (AMI)
An **Amazon Machine Image (AMI)** is a template that contains the software configuration (operating system, application server, and applications) required to launch an EC2 instance. Think of it as a “snapshot” of a server that you can replicate any number of times. 

1. **Components of an AMI**  
   - **Root Volume Template**: This typically includes an OS (e.g., Amazon Linux, Ubuntu, Windows Server) and any pre-installed applications or customizations.  
   - **Launch Permissions**: Defines which AWS accounts can use the AMI to launch EC2 instances. By default, the AMI you create is private to your account.  
   - **Block Device Mapping**: Specifies how the instance’s storage volumes are mapped, including additional EBS volumes or ephemeral instance store volumes.

2. **Types of AMIs**  
   - **Published AMIs by AWS**: Official Amazon Linux, Windows, or other distributions maintained by AWS.  
   - **AWS Marketplace AMIs**: Pre-configured AMIs provided by third-party vendors (e.g., for WordPress, Jenkins, etc.).  
   - **Community AMIs**: AMIs created and shared by the community.  
   - **Custom AMIs**: Created by you or your organization with custom configurations (security patches, software pre-installed, etc.).

3. **Why AMIs Are Important**  
   - **Consistency**: Ensures that every new instance launched has the exact same configuration.  
   - **Scalability**: You can launch multiple instances from a single AMI quickly.  
   - **Disaster Recovery**: If you have a well-prepared AMI, you can quickly spin up a new instance if one fails.  
   - **Customization**: Create AMIs for specific use cases or environments (e.g., a hardened OS with security tools pre-installed).

4. **Advanced Concepts**  
   - **AMI Lifecycle**: AMIs can become outdated, so maintaining versioned AMIs is essential for patching and compliance.  
   - **Region-Specific**: AMIs are region-bound; you can copy an AMI to different regions to facilitate multi-region deployments or backups.  
   - **AMI Creation Automation**: Tools like AWS Systems Manager (Automation runbooks) or Packer can automate AMI creation and updates.

#### Example Use Case
- **Web Server AMI**: Suppose you configure an Ubuntu server with Apache, PHP, and your web application. After testing, you create an AMI. Each new instance you launch from that AMI automatically has Apache and PHP set up, saving you from manual installations each time.

---

### 1.2 Instance Types
**EC2 instance types** determine the hardware configuration and capabilities (CPU, memory, storage, and networking) of your EC2 instances. AWS offers a wide variety of instance families optimized for different use cases.

1. **General Purpose** (e.g., T, M families)  
   - **T-Series (e.g., t2, t3, t4g)**: Burstable performance; great for applications with intermittent workloads.  
   - **M-Series (e.g., m5, m6g)**: Balanced compute, memory, and networking; ideal for many general workloads like small/medium databases or enterprise applications.

2. **Compute Optimized** (e.g., C-Series: c5, c6g)  
   - High CPU-to-memory ratio, suitable for CPU-intensive tasks like batch processing, high-performance computing, or media encoding.

3. **Memory Optimized** (e.g., R-Series: r5, r6g)  
   - Large memory footprints for in-memory databases, big data processing, and high-performance SQL or NoSQL databases.

4. **Storage Optimized** (e.g., I-Series, D-Series)  
   - High, fast local storage (SSD) for workloads requiring low latency, high IOPS (e.g., NoSQL databases, data warehousing).

5. **Accelerated Computing** (e.g., P-Series, G-Series)  
   - Include GPU or FPGA for machine learning, 3D rendering, or other GPU-bound tasks.

6. **Burstable Instances**  
   - T-Series uses CPU credits to “burst” performance above the baseline. Great for small or spiky workloads.

#### Example Use Case
- **Machine Learning Training**: A P3 instance (GPU-optimized) would be suitable to accelerate training of neural networks.  
- **Web Server**: A T3 (burstable) or M5 (general purpose) instance is often sufficient.

---

### 1.3 Key Pairs
A **Key Pair** in AWS consists of a **public key** and a **private key** used to secure SSH or RDP access to your EC2 instances.

1. **Creation of Key Pairs**  
   - **AWS Management Console**: Generate a new key pair in the EC2 console. You download the private key (`.pem` file) which you must keep securely.  
   - **AWS CLI**: Use `aws ec2 create-key-pair --key-name <KeyName>` to create a key pair.

2. **Usage**  
   - **SSH Access (Linux)**: You use the private key (pem file) with `ssh -i <keyfile.pem> ec2-user@<public-ip>` to securely connect.  
   - **RDP Access (Windows)**: AWS uses the private key to decrypt the Windows Administrator password.

3. **Security Best Practices**  
   - Never share your private key; store it securely (e.g., in a password manager or a secure location).  
   - Rotate or revoke key pairs if compromised.  
   - Use different key pairs for different environments (dev, test, prod) for better isolation.

4. **Advanced Considerations**  
   - **EC2 Instance Connect**: AWS offers a browser-based SSH feature that uses temporary keys.  
   - **SSM Session Manager**: Instead of using key pairs, you can use AWS Systems Manager Session Manager to connect to instances, improving security and auditing.

#### Example Use Case
- **Automated Deployments**: Store your private key in a secure environment (like AWS Secrets Manager) and use it within your CI/CD pipeline to SSH into instances and deploy code.

---

## 2. Pricing Models Basics and Launch Templates

### 2.1 Pricing Models Basics
AWS offers multiple **EC2 pricing models** to help you optimize cost based on your workload patterns.

1. **On-Demand**  
   - Pay by the hour or second (depending on instance type) with no long-term commitments.  
   - Best for short-term, unpredictable workloads or for development/test environments.

2. **Reserved Instances (RI)**  
   - Commit to a 1-year or 3-year term for a significantly reduced hourly rate.  
   - Ideal for steady-state or predictable usage (e.g., always-on databases).  
   - Convertible RIs allow changing the instance type within the same family.

3. **Savings Plans**  
   - Similar to RIs but more flexible. You commit to a certain dollar amount per hour, and AWS applies the savings to any eligible compute usage.  
   - Offers up to 66% discount (or more) over On-Demand, depending on your commitment.

4. **Spot Instances**  
   - Bid on unused AWS capacity. Prices fluctuate based on supply and demand.  
   - Can offer up to 90% discount compared to On-Demand.  
   - Instances can be interrupted with a 2-minute warning when AWS needs the capacity back.  
   - Good for fault-tolerant or stateless workloads like batch processing, big data, or CI pipelines.

5. **Dedicated Hosts**  
   - Physical servers fully dedicated to you.  
   - Helps with licensing requirements or compliance constraints.

6. **Dedicated Instances**  
   - Run on hardware dedicated to a single customer but without full host-level visibility that Dedicated Hosts provide.

#### Example Use Case
- **Production Web App**: A combination of Reserved Instances (for steady load) and On-Demand (for spiky traffic) might be optimal.  
- **Batch Processing**: Use Spot Instances to save costs, but ensure your application can handle interruptions.

---

### 2.2 Launch Templates
A **Launch Template** is a resource that captures the launch parameters for an EC2 instance, such as the AMI, instance type, key pair, security groups, and other settings.

1. **Why Use Launch Templates**  
   - **Consistency**: Ensure every instance launched uses a standardized configuration.  
   - **Automation**: Auto Scaling groups, Spot Fleet, and other AWS services can reference launch templates to launch instances automatically.  
   - **Versioning**: Each time you update a launch template, it creates a new version. You can easily roll back or compare versions.

2. **Key Features**  
   - **Multiple Versions**: Each version can have different AMIs or instance types, making it easy to test new configurations.  
   - **Optional and Required Parameters**: You can set default values for parameters but still override them at launch time if needed.

3. **Launch Template vs Launch Configuration**  
   - **Launch Configurations** are older and do not support versioning or advanced features (like T2/T3 credit specification).  
   - **Launch Templates** are recommended for new deployments due to added flexibility and feature support.

4. **Advanced Use Cases**  
   - **Auto Scaling**: Attach a launch template to an Auto Scaling group to automatically create or terminate instances based on scaling policies.  
   - **Spot Fleet**: Define multiple instance types and purchase options in a single template for cost optimization.

#### Example Use Case
- **Auto Scaling a Web Tier**: You create a launch template specifying the AMI (with your web server installed), the instance type (t3.medium), the key pair, security group, and user data script. You then attach it to an Auto Scaling group that scales based on CPU usage.

---

## 3. EBS, EFS, and Instance Store

### 3.1 Amazon EBS (Elastic Block Store)
**Amazon EBS** is a high-performance block storage service designed for use with EC2. It behaves like a raw, unformatted block device.

1. **Types of EBS Volumes**  
   - **General Purpose SSD (gp2/gp3)**: Balanced price/performance for most workloads.  
   - **Provisioned IOPS SSD (io1/io2)**: High-performance for I/O-intensive workloads like large databases.  
   - **Throughput Optimized HDD (st1)**: Low-cost, designed for frequently accessed, throughput-intensive workloads.  
   - **Cold HDD (sc1)**: Lowest cost HDD, for infrequently accessed data.

2. **Key Features**  
   - **Persistence**: Data persists independently of the EC2 instance lifecycle.  
   - **Snapshots**: Point-in-time backups to Amazon S3.  
   - **Encryption**: Native support for encryption at rest using AWS KMS keys.  
   - **Resizing**: You can increase volume size, adjust performance settings, and change volume type on the fly (gp2 to gp3, for example).

3. **Performance Considerations**  
   - **IOPS** (Input/Output Operations Per Second) is crucial for databases.  
   - **Burst Credits** for gp2 volumes if usage is below a certain threshold. gp3 offers baseline IOPS and throughput with optional provisioning for higher performance.

#### Example Use Case
- **Database Server**: Use a Provisioned IOPS (io2) volume for a high-transaction database to ensure low-latency I/O.

---

### 3.2 Amazon EFS (Elastic File System)
**Amazon EFS** is a fully managed, elastic, shared file storage for Linux workloads.

1. **Shared Storage**  
   - Multiple EC2 instances can mount the same EFS file system concurrently, making it ideal for shared data scenarios like web servers or content management systems.

2. **Scalability**  
   - Automatically scales to petabytes of data.  
   - You only pay for what you use (data stored).

3. **Performance Modes**  
   - **General Purpose**: Ideal for latency-sensitive use cases (web servers, CMS).  
   - **Max I/O**: For highly parallel applications requiring high throughput (big data).

4. **Throughput Modes**  
   - **Bursting Throughput**: Scales with file system size.  
   - **Provisioned Throughput**: You can provision a specific throughput if your workload requires it.

5. **Security and Access**  
   - **VPC integration**: Mount targets are created in each Availability Zone you want to access from.  
   - **IAM and NFS Permissions**: You can control access with file-level permissions.

#### Example Use Case
- **Shared Web Content**: If you have multiple EC2 instances in an Auto Scaling group serving a website, they can share the same content repository via EFS.

---

### 3.3 Instance Store
An **Instance Store** provides temporary block-level storage for EC2 instances. It’s physically attached to the host where your instance runs.

1. **Characteristics**  
   - **Ephemeral**: Data is lost when the instance stops, hibernates, or terminates.  
   - **High I/O**: Often provides very high random IOPS because it is directly attached SSD.  
   - **No Persistence**: Not suitable for storing critical data that you can’t afford to lose.

2. **Use Cases**  
   - **Caching or Buffering**: Great for temporary data such as caches, scratch space, or logs that can be regenerated or don’t need long-term storage.  
   - **High-Performance Temporary Storage**: HPC workloads that need extremely fast local disk I/O.

3. **Limitations**  
   - **Cannot be resized**.  
   - **Tied to the instance’s lifecycle**.  
   - **Not all instance types support instance stores** (e.g., T2 does not have an instance store).

#### Example Use Case
- **High-speed Cache**: For a media transcoding server, you can use the instance store for temporary video files before final output is moved to a persistent store like S3.

---

## 4. Snapshots of EBS and EC2

### 4.1 EBS Snapshots
A **Snapshot** is a point-in-time backup of an EBS volume, stored in Amazon S3 (though not directly accessible in your S3 bucket).

1. **Creation**  
   - **Manual**: You can create a snapshot from the AWS Console or CLI at any time.  
   - **Automated**: AWS Data Lifecycle Manager (DLM) can automate snapshot schedules.

2. **Incremental Backups**  
   - The first snapshot is a full backup. Subsequent snapshots are incremental, storing only changed blocks.

3. **Usage**  
   - **Restore**: You can create a new EBS volume from a snapshot.  
   - **Copy Snapshots**: Copy them across regions for disaster recovery or cross-region replication.  
   - **Sharing Snapshots**: You can share snapshots with other AWS accounts (publicly or privately).

4. **Performance Impact**  
   - **Snapshots use I/O** on the volume during creation. It’s best to quiesce the filesystem or take a snapshot during low activity for data consistency.

#### Example Use Case
- **Regular Backups**: A production database on EBS can have daily snapshots scheduled via AWS Data Lifecycle Manager. In case of data corruption, you can restore to a previous snapshot.

---

### 4.2 EC2 Instance Snapshots
- While EBS snapshots are common, you can also create an **AMI** from an existing instance to capture its current state. This is often referred to informally as an “instance snapshot.”  
- This AMI can then be used to launch new instances with the same configuration.

---

## 5. EBS Encryption

### 5.1 What is EBS Encryption?
**EBS encryption** secures your data at rest by encrypting data as it moves between the volume and the EC2 instance. AWS Key Management Service (KMS) manages the cryptographic keys.

1. **Encryption Process**  
   - **AWS-Managed Keys (aws/ebs)**: A default key that AWS manages for you.  
   - **Customer-Managed Keys (CMKs)**: Keys you create and control in KMS.  
   - Data is encrypted on the EBS volume, in transit between the volume and the instance, and in snapshots.

2. **Enabling Encryption**  
   - **Default Encryption**: You can enable default encryption for all new EBS volumes in your account/region.  
   - **Per-Volume**: Specify encryption when creating or attaching a new volume.

3. **Performance Impact**  
   - **Minimal Overhead**: AWS uses hardware-accelerated encryption, so performance impact is typically negligible.

4. **Use Cases**  
   - **Regulatory Compliance**: HIPAA, PCI, or other standards requiring data encryption at rest.  
   - **Security Best Practice**: Encryption is often a recommended baseline practice.

#### Example Use Case
- **Sensitive Data Storage**: If you store PII (personally identifiable information) or financial data on EBS, enabling encryption helps meet compliance requirements.

---

## 6. Elastic IP and Instance Role

### 6.1 Elastic IP Addresses
An **Elastic IP (EIP)** is a static, public IPv4 address that you can associate with an EC2 instance. By default, EC2 instances receive a dynamic public IP which can change if the instance is stopped and started again. An EIP remains yours until you release it.

1. **Why Use an Elastic IP?**  
   - **Static Public IP**: Ensures your external IP doesn’t change, useful for DNS records or if you require a fixed IP for external services.  
   - **Re-Association**: You can move the EIP from one instance to another in case of failover or maintenance.

2. **Costs**  
   - **Free When Attached**: You are charged for EIPs only when they are not associated with a running instance or if you exceed the default limit of one EIP per instance.  
   - **Charge for Idle IP**: Encourages you to release unused EIPs.

3. **Best Practices**  
   - Use EIPs sparingly to avoid unnecessary costs.  
   - Use Route 53 (AWS DNS) or load balancers for more scalable solutions, if possible.

#### Example Use Case
- **Static IP for a Web Server**: If your customers rely on a fixed IP for whitelisting in their firewall, attach an EIP to your instance.

---

### 6.2 Instance Role
An **Instance Role** is an IAM role that an EC2 instance can assume to access AWS services securely, without needing to store credentials on the instance.

1. **How It Works**  
   - **IAM Role**: Defines a set of permissions (policies) for what the instance can do (e.g., read from S3, write to DynamoDB).  
   - **Instance Profile**: Associates the IAM role with the EC2 instance.  
   - **Temporary Credentials**: The instance automatically receives temporary AWS credentials via the metadata service.

2. **Benefits**  
   - **No Hard-Coded Credentials**: Improves security because you don’t embed AWS access keys in your application code.  
   - **Automatic Rotation**: The temporary credentials are rotated automatically.  
   - **Granular Permissions**: You can apply least-privilege policies to limit the scope of what the instance can access.

3. **Advanced Topics**  
   - **Cross-Account Roles**: Let an EC2 instance in one AWS account access resources in another account.  
   - **STS AssumeRole**: You can have an instance role that allows further role assumptions for more complex permission structures.

#### Example Use Case
- **S3 Access from EC2**: Instead of storing an access key in your code, assign an instance role with an S3ReadOnly policy. Your application uses the role to retrieve objects from an S3 bucket.

---

## Putting It All Together:

Imagine you are deploying a high-traffic e-commerce web application:
1. You create a **custom AMI** with the OS patched and your web server installed to ensure consistency across all instances.  
2. You select an **M5 instance type** for a balance of compute and memory.  
3. You secure the instances using a **key pair** that you manage via the AWS console.  
4. You choose a combination of **On-Demand** and **Spot Instances** for your Auto Scaling group to optimize cost.  
5. You define a **Launch Template** with the AMI, instance type, and user data script to automate instance creation.  
6. You store persistent data in **EBS volumes** (gp3) for performance and cost-efficiency, and use **EFS** for shared content.  
7. You schedule **snapshots** of your EBS volumes daily for backups.  
8. You enable **EBS encryption** to comply with data security regulations.  
9. You assign an **Elastic IP** to your primary EC2 instance for a stable IP address that your DNS can point to.  
10. You attach an **instance role** that allows your application to read/write data to other AWS services (e.g., S3, DynamoDB) without hard-coded credentials.

---

## Conclusion

1. **AMI, Instance Types, Key Pairs**  
   - AMIs provide consistent machine images.  
   - Choose the right instance type (T, M, C, R, etc.) for your workload.  
   - Key pairs enable secure SSH/RDP access.

2. **Pricing Models and Launch Templates**  
   - Select a pricing model (On-Demand, RI, Spot) based on workload patterns.  
   - Use Launch Templates for consistent, versioned instance configuration.

3. **EBS, EFS, and Instance Store**  
   - EBS for persistent block storage.  
   - EFS for shared file storage across multiple instances.  
   - Instance Store for high-speed, ephemeral storage.

4. **Snapshots of EBS and EC2**  
   - Snapshots are point-in-time backups.  
   - Automate snapshots for reliable backups and quick recovery.

5. **EBS Encryption**  
   - Encrypt data at rest with AWS-managed or customer-managed keys.  
   - Minimal performance overhead.

6. **Elastic IP and Instance Role**  
   - Elastic IPs provide a static public IP for your instance.  
   - Instance roles enable secure, credential-free access to AWS services.

