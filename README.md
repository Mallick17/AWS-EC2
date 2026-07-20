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

### Level 1: The four strategies, in plain English

**Cluster — "sit together, shoulder to shoulder"**
All instances get packed as physically close together as possible, in a single Availability Zone, ideally on the same network switch. Think of it as seating your whole team at one lunch table so they can pass notes instantly. You get the lowest possible network latency and highest throughput between instances. The tradeoff: if that one "table" (rack) has a problem, everyone sitting there is affected together.
Best for: HPC (high-performance computing), tightly-coupled scientific simulations, MPI workloads, anything where node-to-node chatter is the bottleneck.

**Spread — "everyone gets their own room, no exceptions"**
Each instance is forced onto genuinely separate hardware — different racks, different power, different network gear. It's the opposite philosophy from Cluster: instead of speed, you're buying maximum isolation for a small number of critical instances. Because AWS has to find truly distinct hardware for each one, this strategy is capped at 7 instances per Availability Zone (rack-level spread).
Best for: A handful of must-not-fail-together nodes — e.g., your 3-5 domain controllers, or the leader nodes of a small critical cluster.

**Partition — "small isolated neighborhoods, not one house each"**
This is the middle ground, designed for big distributed systems with dozens or hundreds of nodes. Instances are grouped into logical "partitions" (you choose how many, up to 7 per AZ), and each partition sits on its own racks with its own power and networking — but multiple instances can share hardware within the same partition. AWS even tells your application which instance is in which partition, so topology-aware software (like Hadoop or Cassandra) can be smart about where it puts replicas.
Best for: HDFS, HBase, Cassandra, Kafka — big data and distributed database systems that already understand the concept of "don't put all replicas of the same data in one place."

**Precision Time — the newest strategy (added mid-2026)**
This one is genuinely new and worth flagging since it's easy to miss in older tutorials: AWS added a fourth strategy called **precision-time**, which places your instances on hardware with direct access to a high-precision clock source, giving microsecond-accurate time sync via the enhanced Amazon Time Sync Service. It's aimed at things like financial trading systems and distributed databases that need to order transactions by timestamp with extreme accuracy. You can even attach a precision-time group as a "parent" to a cluster placement group, so you get low latency and precise clocks together. It currently requires newer (Gen7+) instance families.


### Level 2: The rules you'll actually hit

| | Cluster | Spread | Partition | Precision time |
|---|---|---|---|---|
| Scope | Single AZ | Can span AZs in a region | Can span AZs in a region | Depends on hardware |
| Max size | Limited by account quota | 7 instances per AZ | 7 partitions per AZ (unlimited instances) | Limited by supported hardware |
| Dedicated instances/hosts | Allowed | Not allowed | Limited to 2 partitions | N/A |
| Typical use | HPC, low latency | A few critical, isolated nodes | Big distributed systems | Trading systems, distributed DBs needing exact clock order |
| Cost | Free | Free | Free | Free |

A few extra things worth knowing:
- An instance can only be in **one** placement group at a time.
- You can't merge two placement groups together.
- Creating a placement group costs nothing — you only pay for the instances as usual.

### Level 3: Creating one (so it's not just theory)

Through the console: EC2 Dashboard → Network & Security → Placement Groups → Create placement group → pick a strategy → (for Partition, set the number of partitions; for Spread, choose Rack or Host level).

Via CLI:

```bash
# Cluster
aws ec2 create-placement-group --group-name my-cluster --strategy cluster

# Partition, 5 partitions
aws ec2 create-placement-group --group-name HDFS-Group-A --strategy partition --partition-count 5

# Spread
aws ec2 create-placement-group --group-name my-spread --strategy spread

# Precision time (newest strategy)
aws ec2 create-placement-group --group-name my-ptp-group --strategy precision-time
```

Then launch instances with `--placement GroupName=my-cluster`.
