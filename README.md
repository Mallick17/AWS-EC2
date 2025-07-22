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

## **1. Architecture Basics arm64 vs x86_64**

| Feature                                | `x86_64`                                 | `arm64`                                  |
| -------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| **ISA (Instruction Set Architecture)** | CISC (Complex Instruction Set Computing) | RISC (Reduced Instruction Set Computing) |
| **Bit Width**                          | 64-bit                                   | 64-bit                                   |
| **Register Set**                       | More complex and powerful                | Simpler, more streamlined                |

---

## **2. Performance & Efficiency**

| Metric                | `x86_64`                                    | `arm64`                                                           |
| --------------------- | ------------------------------------------- | ----------------------------------------------------------------- |
| **Power Efficiency**  | Higher power consumption                    | Very power-efficient                                              |
| **Performance (raw)** | Generally faster for legacy, high-end tasks | Often competitive, especially for modern, multithreaded workloads |
| **Thermal Output**    | Higher heat generation                      | Runs cooler                                                       |

---

## **3. Compatibility**

| Feature                | `x86_64`                           | `arm64`                                         |
| ---------------------- | ---------------------------------- | ----------------------------------------------- |
| **Software Ecosystem** | Mature (especially legacy/desktop) | Growing rapidly (esp. in cloud & mobile)        |
| **Linux/Unix support** | Extensive                          | Excellent, especially in cloud (AL2023, Ubuntu) |
| **Windows support**    | Fully supported                    | Still improving                                 |
| **Docker containers**  | Architecture-specific containers   | Needs `arm64` images or emulation (e.g., QEMU)  |

---

## **4. Cloud and AWS**

| Category                   | `x86_64`                                   | `arm64`                                            |
| -------------------------- | ------------------------------------------ | -------------------------------------------------- |
| **AWS EC2 Instance Types** | Most instance families (e.g., `m5`, `c6i`) | Graviton/Graviton2/Graviton3 (`t4g`, `c7g`, `m7g`) |
| **Performance/Price**      | Standard baseline                          | Often **\~20–40% cheaper** per performance unit    |
| **Ideal for**              | Legacy apps, high-performance apps         | Cloud-native apps, microservices, containers       |

---

## **5. Real-World Use Cases**

| Use Case                   | Best Architecture    |
| -------------------------- | -------------------- |
| Mobile Devices             | ✅ `arm64`            |
| Legacy Enterprise Software | ✅ `x86_64`           |
| High-Performance Compute   | ✅ `x86_64` (for now) |
| Cloud-native Workloads     | ✅ `arm64` (Graviton) |
| Battery-sensitive Devices  | ✅ `arm64`            |

---

## Example in AWS AMIs

If you're using:

* `al2023-ami-ecs-hvm-<date>-kernel-6.1-**x86_64**`: Use with Intel/AMD (e.g., `c6i`, `m5`)
* `al2023-ami-ecs-hvm-<date>-kernel-6.1-**arm64**`: Use with Graviton (`c7g`, `t4g`, etc.)

---

## Summary

| Want...                         | Choose... |
| ------------------------------- | --------- |
| **Best compatibility**          | `x86_64`  |
| **Lowest cost, cloud-native**   | `arm64`   |
| **Mobile, IoT, or battery use** | `arm64`   |
| **High raw compute power**      | `x86_64`  |

---

# AWS Instance Metadata Service v2 (IMDSv2): Comprehensive Guide

## 1. What is Instance Metadata Service (IMDS)?

The **Instance Metadata Service (IMDS)** allows applications on AWS EC2 instances to access information about the instance environment—such as ID, networking details, attached roles, user data, and more—via HTTP requests to a special local endpoint (`169.254.169.254`). This is essential for automation, bootstrapping, and dynamically managing cloud infrastructure.

## 2. IMDSv1 vs. IMDSv2: Key Differences

| Feature                   | IMDSv1 (Legacy)              | IMDSv2 (Modern, Secure)          |
|---------------------------|------------------------------|----------------------------------|
| Authentication            | None                         | Requires session token           |
| Access Method             | HTTP GET only                | Session-token workflow           |
| SSRF Risk                 | High                         | Low                              |
| Backwards Compatibility   | Legacy (no code changes)     | Scripts must be token-aware      |
| Recommended?              | No (deprecated/insecure)     | Yes (default, enforced by AWS)   |

## 3. IMDSv2: How It Works

### Session Token Mechanism

IMDSv2 introduces a **two-step, session-oriented process**:

1. **Request a Session Token**
   - Must be done via a PUT request from inside the instance.
   - Token validity is user-defined (up to 6 hours per session).

   **Example:**
   ```sh
   TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" \
     -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
   ```

2. **Make Metadata Requests With the Token**
   - Pass the token in the header for all subsequent metadata queries.

   **Example:**
   ```sh
   curl -H "X-aws-ec2-metadata-token: $TOKEN" \
     http://169.254.169.254/latest/meta-data/instance-id
   ```

**Note:** Each request must supply the valid token; scripts must be updated from legacy GETs to request, store, and use the token appropriately.

## 4. What Metadata Can You Access?

With proper IMDSv2 session token use, you can securely access:

- **Instance Identity:** Instance ID, AMI ID, type
- **Networking:** Private/Public IP, MAC, VPC, Subnet, Security Groups
- **IAM Credentials:** Role, temporary keys/tokens
- **User Data:** Bootstrapping/script data (if provided at launch)
- **Tags:** All attached tags (if "allow tags in metadata" is enabled)
- **Storage:** Volume attachments, device mappings
- **Other:** Placement/group info, scheduled events

### Example: Fetch Instance ID

**Legacy (IMDSv1):**
```sh
curl http://169.254.169.254/latest/meta-data/instance-id
```

**With IMDSv2:**
```sh
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" \
  -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
curl -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/instance-id
```

## 5. Configuring IMDSv2 on EC2 Instances

### Key Configuration Options

| Setting                       | Recommendation                          | Description                                  |
|-------------------------------|-----------------------------------------|----------------------------------------------|
| Metadata accessible           | Enabled (default)                       | Allows apps/scripts to reach metadata        |
| Metadata version              | V2 only (token required)                | Blocks vulnerable IMDSv1 access              |
| Metadata response hop limit   | 1 (default), set to 2 for containers    | Higher for container workloads needing IMDS  |
| Allow tags in metadata        | Enable for automation                   | Makes tags available in metadata responses   |
| User data script              | As needed for bootstrapping             | Runs at launch, can use IMDSv2 tokens        |

**Enforce IMDSv2 on an instance:**
```sh
aws ec2 modify-instance-metadata-options \
  --instance-id  \
  --http-tokens required \
  --http-endpoint enabled
```

## 6. Use Cases and Security Guidance

### When to Use IMDSv2

- **Highly recommended for all modern cloud workloads:** Prevents common attacks such as SSRF and metadata credential theft.
- **Required for container workloads** (e.g., ECS/EKS) with hop limit increases.

### When to Restrict or Disable

- **If no in-instance software requires metadata** and your setup mandates minimal exposure, the metadata endpoint can be disabled.
- **When legacy tooling is present,** migrate or update all accesses to use the IMDSv2 session-token workflow.

## 7. Pros & Cons

| Pros                                           | Cons                          |
|------------------------------------------------|-------------------------------|
| Strong security (blocks SSRF, leaks, etc.)     | Legacy tools/scripts break if not updated |
| Enables automation and dynamic config          | Slight increase in code/script complexity   |
| Tag availability for self-aware resources      | Migration must be planned/tested            |
| Required for modern AWS best-practices         | None (unless old patterns remain) |

## 8. Migration Checklist

1. **Audit all scripts/applications accessing metadata** – Update them to use the token-fetch pattern.
2. **Enforce IMDSv2** (`http-tokens=required`) only after all tooling is compliant.
3. **Test instance launch and configuration** with IMDSv2 required; verify application functionality.
4. **Optionally enable tags in metadata** for greater automation potential.

## 9. Summary Table: IMDSv1 vs. IMDSv2 Request Flow

| Step                        | IMDSv1 Command                                 | IMDSv2 Command                                   |
|-----------------------------|------------------------------------------------|--------------------------------------------------|
| Metadata call               | `curl http://169.254.169.254/latest/meta-data/instance-id` | `curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id` |
| Token retrieval             | _(none)_                                       | `TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds:21600")` |

## 10. Best Practices

- **Require IMDSv2 on all workloads for maximum security.**
- **Update all metadata access patterns to request and use session tokens.**
- **Set hop limit to 2 only for containerized environments; otherwise, keep at 1.**
- **Monitor metadata accesses for audits and incident response.**
- **Enable tags in metadata for improved automation if tagging policy is standardized.**
<img width="1520" height="754" alt="image" src="https://github.com/user-attachments/assets/8d3f8d84-9e1b-40e8-85d4-bf28505911fc" />
