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

## How it is charged in EC2 by separating all the components
If an **Amazon Machine Image (AMI)** is charged separately, then **Amazon Elastic Block Store (EBS)** and **other related services** are usually **charged separately** as well. Here's a breakdown of the main AWS charges associated with EC2 and AMIs:

### 🧾 **1. AMI Charges**

* **Free AMIs:** Most Amazon-provided AMIs (like Amazon Linux, Ubuntu, etc.) are free — you just pay for the EC2 and other resources.
* **Paid/Marketplace AMIs:** Some AMIs from AWS Marketplace include software or services (e.g., antivirus, analytics tools) and incur **separate hourly or monthly fees**.

---

### 💾 **2. EBS (Elastic Block Store) Charges**

Charged separately based on:

* **Volume size & type:** (e.g., gp3, io2)
* **Provisioned IOPS (if applicable)**
* **Snapshot storage** (for backups)
* **Data transfer (EBS to another AZ/region)**

---

### 💻 **3. EC2 Instance Charges**

* Based on **instance type**, region, and **running time**
* Pricing models: **On-Demand**, **Reserved**, **Spot**, or **Savings Plans**

---

### 🌐 **4. Data Transfer Costs**

* **Intra-region data transfer:** Often free or low-cost
* **Inter-region or Internet data transfer:** Charged based on GBs
* **Public IP traffic:** Billed separately for data going out to the internet

---

### 📸 **5. Snapshots (Backups)**

* Charged per **GB/month** of snapshot storage
* Also charged when **restoring** snapshots to volumes

---

### 🔧 **6. Other Common Charges**

* **Elastic IPs (EIP):** Free while associated with a running instance; charged if not in use
* **Load Balancers (ELB):** Charged per hour and per GB
* **CloudWatch:** Basic monitoring is free; detailed metrics and custom dashboards incur charges
* **VPC NAT Gateways:** Per hour + data processing
* **Support Plans:** If enabled, AWS Support is a separate subscription

---

### Example: Running a Paid AMI

If you launch an EC2 instance with a paid AMI:

1. You pay the **AMI license fee (per hour/month)**
2. You pay for the **EC2 instance (CPU, RAM, etc.)**
3. You pay for the **EBS volume** (root + any attached)
4. You may pay for **data transfer** (especially out to internet)
5. You may incur **snapshot** or **CloudWatch** costs if enabled

---
