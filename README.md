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
