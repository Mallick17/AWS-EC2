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

### Introduction to EC2 Instance Types
When launching an EC2 instance, the instance type you select shapes the host computer's hardware, offering varying levels of compute, memory, storage, and networking. This choice is crucial for matching your application's needs, ensuring optimal performance and cost efficiency.

### Instance Type Categories and Examples
AWS categorizes EC2 instances to fit diverse use cases. Below are key categories with examples and specifications:

#### General Purpose Instances
These provide a balance of resources, ideal for web servers and code repositories. They include fixed performance (e.g., M5) and burstable performance (e.g., T3) instances, as well as flex instances (e.g., M7i-flex).

- **M5 Instances (Fixed Performance):**
  | Instance Type | vCPUs | Memory (GiB) | Network Performance |
  |---------------|-------|--------------|---------------------|
  | m5.large      | 2     | 8            | Up to 10 Gbps       |
  | m5.xlarge     | 4     | 16           | Up to 10 Gbps       |
  | m5.2xlarge    | 8     | 32           | Up to 10 Gbps       |

- **T3 Instances (Burstable Performance):**
  | Instance Type | vCPUs | Memory (GiB) | Baseline CPU Performance |
  |---------------|-------|--------------|--------------------------|
  | t3.medium     | 2     | 4            | 20%                      |
  | t3.large      | 2     | 8            | 30%                      |

- **Flex Instances (e.g., M7i-flex):** Offer a balance of resources, with the ability to burst up to 100% CPU for 95% of a 24-hour period, suitable for general workloads.

#### Compute Optimized Instances
Designed for compute-intensive tasks like batch processing and scientific modeling, these instances, such as C5, provide high-performance processors.

- **C5 Instances:**
  | Instance Type | vCPUs | Memory (GiB) | Network Performance |
  |---------------|-------|--------------|---------------------|
  | c5.large      | 2     | 4            | Up to 10 Gbps       |
  | c5.xlarge     | 4     | 8            | Up to 10 Gbps       |
  | c5.2xlarge    | 8     | 16           | Up to 10 Gbps       |

#### Memory Optimized Instances
These are for workloads processing large datasets in memory, like databases and real-time big data, with examples like R5 offering high memory capacity.

- **R5 Instances:**
  | Instance Type | vCPUs | Memory (GiB) | Network Performance |
  |---------------|-------|--------------|---------------------|
  | r5.large      | 2     | 16           | Up to 10 Gbps       |
  | r5.xlarge     | 4     | 32           | Up to 10 Gbps       |
  | r5.2xlarge    | 8     | 64           | Up to 10 Gbps       |

#### Storage Optimized Instances
Optimized for high I/O access to large datasets, suitable for NoSQL databases and data warehousing, with examples like I3 offering NVMe SSD storage.

- **I3 Instances:**
  | Instance Type | vCPUs | Memory (GiB) | Storage (GB) | Network Performance |
  |---------------|-------|--------------|--------------|---------------------|
  | i3.large      | 2     | 15.25        | 475 NVMe SSD | Up to 10 Gbps       |
  | i3.xlarge     | 4     | 30.5         | 950 NVMe SSD | Up to 10 Gbps       |

#### Accelerated Computing Instances
These use hardware accelerators like GPUs for graphics processing and machine learning, with examples like P3 offering NVIDIA Tesla V100 GPUs.

- **P3 Instances:**
  | Instance Type | vCPUs | Memory (GiB) | GPUs | GPU Memory (GiB) | Network Performance |
  |---------------|-------|--------------|------|------------------|---------------------|
  | p3.2xlarge    | 8     | 61           | 1    | 16               | 10 Gbps             |
  | p3.8xlarge    | 32    | 244          | 4    | 64               | 25 Gbps             |

### Additional Details
- **Burstable Performance:** Managed by CPU credits, earned below baseline and spent when bursting, ideal for variable CPU usage.
- **Flex Instances:** Like M7i-flex and C7i-flex, balance cost and performance, bursting up to 100% CPU for 95% of 24 hours.

---

<details>
   <summary>Comprehensive Analysis of AWS EC2 Instance Types</summary>

### Comprehensive Analysis of AWS EC2 Instance Types

This detailed report expands on the key points, providing a thorough exploration of AWS EC2 instance types, their categories, and specifications, tailored for a DevOps intern beginning with AWS EC2. The analysis is based on provided documentation and additional resources, ensuring a complete understanding for practical application.

#### Background and Context
AWS EC2 (Elastic Compute Cloud) instance types define the hardware capabilities of virtual servers, including compute, memory, storage, and networking. Each instance type is grouped into families based on resource balance, catering to diverse application needs. The choice impacts performance, cost, and scalability, making it essential for DevOps professionals to understand these options.

#### Detailed Categorization and Use Cases
EC2 instances are categorized to align with specific workloads, as outlined below, with examples and specifications drawn from the provided data.

##### General Purpose Instances
General purpose instances offer a balanced mix of compute, memory, and networking, suitable for web servers, code repositories, and small databases. They include fixed performance instances (e.g., M5), burstable performance instances (e.g., T3), and flex instances (e.g., M7i-flex).

- **Fixed Performance (M5 Instances):**
  M5 instances, powered by Intel Xeon Platinum 8175 processors, are designed for demanding workloads. Specifications include:
  | Instance Type | vCPUs | Memory (GiB) | Network Performance |
  |---------------|-------|--------------|---------------------|
  | m5.large      | 2     | 8            | Up to 10 Gbps       |
  | m5.xlarge     | 4     | 16           | Up to 10 Gbps       |
  | m5.2xlarge    | 8     | 32           | Up to 10 Gbps       |
  Network specifications from the attachment show baseline bandwidth starting at 0.75 Gbps for m5.large, bursting to 10 Gbps, highlighting their capability for general applications.

- **Burstable Performance (T3 Instances):**
  T3 instances, part of the burstable family, provide a baseline CPU performance with burst capability, managed by CPU credits. They are cost-effective for applications with variable CPU needs, such as development environments. Specifications include:
  | Instance Type | vCPUs | Memory (GiB) | Baseline CPU Performance |
  |---------------|-------|--------------|--------------------------|
  | t3.medium     | 2     | 4            | 20%                      |
  | t3.large      | 2     | 8            | 30%                      |
  The attachment confirms T3 uses Intel Skylake P-8175, with no accelerators, focusing on flexibility.

- **Flex Instances (M7i-flex, C7i-flex):**
  Flex instances, like M7i-flex, offer a cost-effective balance, with reliable CPU at 40% baseline, bursting to 100% for 95% of a 24-hour period. They are ideal for general-purpose workloads, with specifications similar to M5 but optimized for cost.

##### Compute Optimized Instances
Compute optimized instances, such as C5, are tailored for compute-intensive tasks like batch processing and media transcoding, leveraging high-performance processors. Specifications include:
  | Instance Type | vCPUs | Memory (GiB) | Network Performance |
  |---------------|-------|--------------|---------------------|
  | c5.large      | 2     | 4            | Up to 10 Gbps       |
  | c5.xlarge     | 4     | 8            | Up to 10 Gbps       |
  | c5.2xlarge    | 8     | 16           | Up to 10 Gbps       |
  Powered by Intel Xeon Platinum 8124M, these instances ensure consistent performance, with no burstable options, as confirmed by the attachment.

##### Memory Optimized Instances
Memory optimized instances, like R5, are designed for in-memory processing, suitable for databases and real-time analytics. Specifications include:
  | Instance Type | vCPUs | Memory (GiB) | Network Performance |
  |---------------|-------|--------------|---------------------|
  | r5.large      | 2     | 16           | Up to 10 Gbps       |
  | r5.xlarge     | 4     | 32           | Up to 10 Gbps       |
  | r5.2xlarge    | 8     | 64           | Up to 10 Gbps       |
  Using Intel Xeon Platinum 8175, R5 instances offer high memory capacity, with no accelerators, focusing on memory-intensive tasks.

##### Storage Optimized Instances
Storage optimized instances, such as I3, are for workloads requiring high I/O, like NoSQL databases. Specifications include:
  | Instance Type | vCPUs | Memory (GiB) | Storage (GB) | Network Performance |
  |---------------|-------|--------------|--------------|---------------------|
  | i3.large      | 2     | 15.25        | 475 NVMe SSD | Up to 10 Gbps       |
  | i3.xlarge     | 4     | 30.5         | 950 NVMe SSD | Up to 10 Gbps       |
  These instances provide NVMe SSD storage, optimized for low-latency, high-throughput operations.

##### Accelerated Computing Instances
Accelerated computing instances, like P3, use GPUs for tasks like machine learning. Specifications include:
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
