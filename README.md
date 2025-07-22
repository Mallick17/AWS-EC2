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
