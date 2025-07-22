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

### 1. What is Instance Metadata Service (IMDS)?

The **Instance Metadata Service (IMDS)** allows applications on AWS EC2 instances to access information about the instance environment—such as ID, networking details, attached roles, user data, and more—via HTTP requests to a special local endpoint (`169.254.169.254`). This is essential for automation, bootstrapping, and dynamically managing cloud infrastructure.

### 2. IMDSv1 vs. IMDSv2: Key Differences

| Feature                   | IMDSv1 (Legacy)              | IMDSv2 (Modern, Secure)          |
|---------------------------|------------------------------|----------------------------------|
| Authentication            | None                         | Requires session token           |
| Access Method             | HTTP GET only                | Session-token workflow           |
| SSRF Risk                 | High                         | Low                              |
| Backwards Compatibility   | Legacy (no code changes)     | Scripts must be token-aware      |
| Recommended?              | No (deprecated/insecure)     | Yes (default, enforced by AWS)   |

### 3. IMDSv2: How It Works

#### Session Token Mechanism

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

### 4. What Metadata Can You Access?

With proper IMDSv2 session token use, you can securely access:

- **Instance Identity:** Instance ID, AMI ID, type
- **Networking:** Private/Public IP, MAC, VPC, Subnet, Security Groups
- **IAM Credentials:** Role, temporary keys/tokens
- **User Data:** Bootstrapping/script data (if provided at launch)
- **Tags:** All attached tags (if "allow tags in metadata" is enabled)
- **Storage:** Volume attachments, device mappings
- **Other:** Placement/group info, scheduled events

#### Example: Fetch Instance ID

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

### 5. Configuring IMDSv2 on EC2 Instances

#### Key Configuration Options

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

### 6. Use Cases and Security Guidance

#### When to Use IMDSv2

- **Highly recommended for all modern cloud workloads:** Prevents common attacks such as SSRF and metadata credential theft.
- **Required for container workloads** (e.g., ECS/EKS) with hop limit increases.

#### When to Restrict or Disable

- **If no in-instance software requires metadata** and your setup mandates minimal exposure, the metadata endpoint can be disabled.
- **When legacy tooling is present,** migrate or update all accesses to use the IMDSv2 session-token workflow.

### 7. Pros & Cons

| Pros                                           | Cons                          |
|------------------------------------------------|-------------------------------|
| Strong security (blocks SSRF, leaks, etc.)     | Legacy tools/scripts break if not updated |
| Enables automation and dynamic config          | Slight increase in code/script complexity   |
| Tag availability for self-aware resources      | Migration must be planned/tested            |
| Required for modern AWS best-practices         | None (unless old patterns remain) |

### 8. Migration Checklist

1. **Audit all scripts/applications accessing metadata** – Update them to use the token-fetch pattern.
2. **Enforce IMDSv2** (`http-tokens=required`) only after all tooling is compliant.
3. **Test instance launch and configuration** with IMDSv2 required; verify application functionality.
4. **Optionally enable tags in metadata** for greater automation potential.

### 9. Summary Table: IMDSv1 vs. IMDSv2 Request Flow

| Step                        | IMDSv1 Command                                 | IMDSv2 Command                                   |
|-----------------------------|------------------------------------------------|--------------------------------------------------|
| Metadata call               | `curl http://169.254.169.254/latest/meta-data/instance-id` | `curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id` |
| Token retrieval             | _(none)_                                       | `TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds:21600")` |

### 10. Best Practices

- **Require IMDSv2 on all workloads for maximum security.**
- **Update all metadata access patterns to request and use session tokens.**
- **Set hop limit to 2 only for containerized environments; otherwise, keep at 1.**
- **Monitor metadata accesses for audits and incident response.**
- **Enable tags in metadata for improved automation if tagging policy is standardized.**

## 📝 Explanation: EC2 User Data Script — What It Does and Why It Matters

Here's a clear breakdown of the **code provided** that’s placed inside the **EC2 user data script**, followed by:

- ✅ What happens **if you include** the script
- ⚠️ What happens **if you don’t include** the script
- 🎯 Why each line is important
- 🚨 Security and operational implications

### 📦 The Full Script (Clean Format):

```bash
yum install -y wget awscli

mkdir ~/.aws
touch ~/.aws/credentials
chmod -R 777 ~/.aws

echo "[default]" > ~/.aws/credentials
echo "region = ap-south-1" >> ~/.aws/credentials

aws ec2 disassociate-address --public-ip 13.126.81.250

TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" \
  -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")

INSTANCE_ID=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/instance-id)

echo "$INSTANCE_ID"

aws ec2 associate-address --instance-id "$INSTANCE_ID" \
  --allocation-id eipalloc-0ab5b6799a11dc742
```

---

### 🔍 Line-by-Line Breakdown

#### 1. `yum install -y wget awscli`
- **Purpose:** Installs `wget` and the AWS CLI so the instance can run AWS commands (like `associate-address`).
- **If missing:** The rest of the script will fail due to missing dependencies.

#### 2. Configuring AWS Credentials (region only, without keys):

```bash
mkdir ~/.aws
touch ~/.aws/credentials
chmod -R 777 ~/.aws
echo "[default]" > ~/.aws/credentials
echo "region = ap-south-1" >> ~/.aws/credentials
```

- **Purpose:** Creates a minimal AWS CLI config with the default region set.
- 🛡 **Important:** No access keys are stored—assumes IAM role is attached to the EC2 instance.
- **If missing:** Some AWS CLI commands might fail if the region must be explicitly provided and isn't set.

#### 3. Disassociate static public IP:

```bash
aws ec2 disassociate-address --public-ip 13.126.81.250
```

- **Purpose:** Releases the current association of a static Elastic IP (EIP) from **another instance** (or possibly the same one).
- Useful when reassigning a shared EIP to a new EC2 (e.g., in blue-green deployments or failover scenarios).
- **If missing:** The associate call later might fail due to EIP still being attached elsewhere.

#### 4. IMDSv2 — Securely get instance metadata:

```bash
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" \
  -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
INSTANCE_ID=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/instance-id)
```

- **Purpose:** Securely fetch the current instance’s ID using **IMDSv2** (token-based).
- Compliant with AWS best practices and avoids legacy IMDSv1 vulnerabilities.
- **If missing:** Instance ID won’t be fetched—or worse, if you use IMDSv1 style but IMDSv2 is enforced (as you are doing), the script FAILS with 401 error.

#### 5. Associate EIP with this instance:

```bash
aws ec2 associate-address --instance-id "$INSTANCE_ID" \
  --allocation-id eipalloc-0ab5b6799a11dc742
```

- **Purpose:** Assigns the specific Elastic IP to the current instance.
- Used for restoring public access, migrating endpoints, or switching production traffic quickly during deployment.
- **If missing:** Your instance will not be reachable via the expected static IP. This can break DNS, traffic routing, or fail safe cutover.

### ✅ What Happens If You **Do Add** This Code to User Data

| Outcome                             | Benefit                                                                 |
|-------------------------------------|-------------------------------------------------------------------------|
| 🛰 AWS CLI Installed                | Required for interacting with EC2 APIs from inside the instance.       |
| 📦 Region Configured               | Ensures CLI doesn't throw missing region errors.                        |
| 🔌 EIP Reassignment Automated      | Fully automates public IP handover to the new instance.                |
| 🧾 IMDSv2 Token Usage Compliant    | Compatible with your **‘IMDSv2-only’** instance metadata config.        |
| 🚀 Zero-Touch Bootstrapping        | Instance is fully operational with correct public IP on launch.        |

### ⚠️ What If You **Do Not Add** This Code?

| Missing Action                                      | Effect / Problem                                                                                  |
|-----------------------------------------------------|----------------------------------------------------------------------------------------------------|
| No CLI or region config                             | AWS commands fail—script breaks.                                                                   |
| No disassociate or associate                        | Your instance won't receive the correct public IP.                                                 |
| No token request                                    | Fails if instance metadata is set to **IMDSv2 only**, which you’ve already enforced.              |
| No auto-mapping of EIP                              | Manual steps needed; may cause downtime or broken routing.                                         |
| No bootstrap automation                             | Instance is just online—not traffic-ready.                                                         |

### 🎯 Why This Script Matters

This script ensures that:
- Your EC2 instance **automatically takes control of a specific Elastic IP**.
- All metadata calls are made **securely using IMDSv2** tokens.
- The instance is **production-ready immediately after launch.**

This is especially useful in:

- ✅ **Auto-scaling groups** where EIPs must be reassigned
- ✅ **Failover scenarios** (high availability setups)
- ✅ **Blue-Green deployments**
- ✅ **Cost-saving ECS clusters** where instances replace each other dynamically

### 🛡 Security & Best Practices

- ✔ Uses **IMDSv2-compliant** metadata requests — protects against SSRF and unauthorized access.
- ✔ Uses **IAM roles**, not hard-coded credentials — secure and maintainable.
- ⚠️ Consider **restricting permission** for `ec2:AssociateAddress` to fixed EIP & instance-role.
- ⚠️ Be cautious with `chmod -R 777 ~/.aws` — fast but permissive. Refine if working with secure environments.

### ✔️ Summary

| With Script                        | Without Script                          |
|-----------------------------------|-----------------------------------------|
| ✅ Instance auto-bootstraps EIP   | ❌ Manual reassignment needed           |
| ✅ Supports IMDSv2 securely       | ❌ Metadata access will fail (HTTP 401) |
| ✅ CLI works with set config      | ❌ Commands fail if region not set      |
| ✅ Ideal for automation/DevOps    | ❌ Fails cloud-ready reliability goals |

**✅ Recommendation:** YES — Add this script if:
- You require the instance to auto-configure itself with a static EIP
- You're using IMDSv2-only (as enforced by your settings)
- You want zero-touch, secure bootstrapping and remote access availability

---

## Effects of Enforcing IMDSv2 Without Bootstrapping Scripts

When you **enable IMDSv2** but do **not add startup scripts** (such as for fetching tokens, auto-mapping Elastic IPs, or automated initialization), your instance will behave differently depending on your operational needs. Below is a breakdown addressing the scenarios and whether the given script is required if you don’t use Elastic IPs.

### 1. What Happens With IMDSv2 Enabled but No Scripts

#### Advantages

- **Improved Security**
  - IMDSv2 enforces token-based metadata access, blocking classic SSRF attacks and helping prevent unauthorized credential leaks.
- **Lower Attack Surface**
  - By requiring a session token, it’s much harder for malicious code inside your instance to fetch metadata.

#### Disadvantages

- **No Automated Bootstrapping**
  - Without user data scripts, your instance will *not* auto-configure itself (e.g., joining clusters, registering with orchestration tools, setting special tags).
- **No EIP Management**
  - If Elastic IP assignment is not automated, manual steps are required for failover, migration, or recovery scenarios.
- **Metadata Access Issues**
  - Any legacy scripts, agents, or services that weren’t updated for IMDSv2 (i.e., still using direct GET requests without a token) will fail to access instance metadata, potentially breaking automation, monitoring, or security tools.

### 2. When You **Don’t Use Elastic IPs**

If your deployment **does not require assigning or reassigning Elastic IPs**:

- **No Need for EIP-Related Script Sections**
  - You can safely omit all commands relating to disassociating or associating Elastic IPs. These commands are specifically for scenarios where your instance must be reachable through a consistent public IP, such as blue-green deployments or hot failover.
- **What Remains Essential**
  - You only need user data scripts if your application requires some kind of custom bootstrapping (e.g., auto-registration, config initialization). If your instance can come up “vanilla” and doesn’t require immediate automation beyond what’s already on the image/AMI, you do not need the script.
- **Security and Compliance**
  - Still ensure any scripts, tools, or third-party software accessing instance metadata have been updated to use the IMDSv2 token approach.

### 3. Summary Table: With and Without the Script

| Scenario                                                | If Script Is Added                                                      | If Script Is Not Added                                  |
|---------------------------------------------------------|------------------------------------------------------------------------|---------------------------------------------------------|
| IMDSv2 enforcement only                                 | Tools/scripts using IMDSv2 will work if token logic is present.         | Legacy metadata calls fail (401 error); better security.|
| EIP management needed                                   | Instance auto-assigns/reassigns Elastic IP.                             | Requires manual EIP change; risk of downtime.           |
| EIP management **not** needed (as in your case)         | EIP logic in script is redundant/unnecessary.                           | No negative impact; script not required.                |
| Bootstrapping or registration (non-EIP) required        | Runs automatically at launch.                                           | Must be handled manually or via AMI image.              |
| Security posture                                        | Improved, as IMDSv2 blocks legacy access and script runs securely.      | Security still improved, but less automation.           |

### 4. Takeaway

- **If you do not use Elastic IPs** and have no other custom bootstrapping requirements, you do **not need** the script shown earlier.
- **If you do need automatic registration, config, or integration,** use a script—but ensure all IMDSv2 calls include a session token.
- **Enforcing IMDSv2** alone is a strong security move, but requires revising any code or tools that attempt to access instance metadata directly.

---
