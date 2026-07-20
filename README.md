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

## EC2 Placement Groups
Placement groups are AWS's way of letting you tell EC2 "control where you physically put my servers" instead of just accepting wherever AWS decides. Here's the same concept, layered from complete beginner to what you need to know in a production interview or a real deployment.

### Level 0: The problem it solves

Normally, when you launch an EC2 instance, AWS just picks a physical server somewhere in its data center for you — you have zero say in it. Most of the time that's fine. But two very different problems come up:

- **"I need my servers to talk to each other REALLY fast"** — think a supercomputing cluster where milliseconds of network delay ruin the math.
- **"I need my servers to NOT fail together"** — if they're all secretly running on the same physical rack, one power supply dying could take down every one of them at once, defeating the whole point of having multiple servers.

A placement group is you telling AWS "hey, for this group of instances, use this specific strategy for deciding where to physically put them." The diagram above shows the three classic strategies in action — same rack, separate racks, or grouped-but-separated racks.
