# Elastic IP
### **Definition and Purpose:**
- An Elastic IP is like a fixed public address you can assign to your EC2 instance in AWS. It stays with your account until you release it, so if your instance fails, you can quickly switch it to another one without changing the address. This is great for keeping your website or service online with the same address, even if something goes wrong.
- An Elastic IP address is a static, public IPv4 address designed for dynamic cloud computing, allocated to your AWS account until explicitly released. It can be associated with an EC2 instance or network interface, providing a persistent public endpoint. This is particularly useful for masking instance failures by remapping the address to another instance, ensuring continuity for applications requiring a fixed IP, such as web servers or APIs.

### **Key Characteristics:**
- **Allocation and Association:** You first allocate an Elastic IP from AWS, then associate it with an instance or network interface. It remains allocated until released, and you can disassociate and reassociate it as needed.
- **Public IPv4:** Reachable from the internet, enabling communication for instances without public IPv4 addresses. It replaces the default public IP when associated, and the instance receives a public DNS hostname if DNS hostnames are enabled ([DNS attributes for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html)).
- **Region-Specific:** Cannot be moved between regions, tied to the region of allocation.
- **Pricing and Quota:** Charged for all public IPv4 addresses, including Elastic IPs not associated with running instances ([Amazon VPC pricing](https://aws.amazon.com/vpc/pricing/)). Default quota is 5 per region, with increases available via Service Quotas console ([Amazon EC2 service quotas](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-resource-limits.html)).
- **Source Options:** Can be from Amazon's IPv4 pool, a customer-owned pool, or a pool brought to your account, with IPv6 not supported.

### **How It Works:**
- To use an Elastic IP, allocate it via the AWS Management Console, CLI, or SDK, then associate it with an instance. For example, using the CLI: `aws ec2 allocate-address --domain vpc` allocates an address, and `aws ec2 associate-address --instance-id i-1234567890abcdef0 --allocation-id eipalloc-1234567890abcdef0` associates it. If the instance fails, disassociate and reassociate with a new instance, ensuring minimal downtime.
- You allocate an Elastic IP from AWS, link it to your instance, and it acts as the public face for internet access. If the instance stops or fails, you can remap it to a new instance, ensuring continuity. It’s especially useful for DNS records, so your domain always points to the right place.

### **Use Cases:**
- **Web Hosting:** Maintain a consistent IP for DNS records, ensuring domain resolution remains stable during instance changes.
- **Failover Management:** Mask instance failures by remapping to a standby instance, enhancing availability.
- **External Service Integration:** Use in firewall rules or external service configurations, avoiding updates when instances scale or replace.
- It’s a public IPv4 address, reachable from the internet.
- There’s a small charge if it’s not linked to a running instance, so remember to release unused ones.
- By default, you get 5 per region, but you can ask for more if needed.

### **Best Practices:**
- Release unused Elastic IPs to avoid charges, as they accrue costs when not associated with running instances.
- Monitor usage via AWS Cost Explorer to optimize costs.
- Use tags for organization and cost allocation, with max 32 characters, alphanumeric and special characters (+ - = . _ : / @).

### **Practical Example:**
Allocate an Elastic IP for a web server:
- Open EC2 console at [https://console.aws.amazon.com/ec2/](https://console.aws.amazon.com/ec2/).
- Navigate to Elastic IPs, choose Allocate Elastic IP address.
- Select VPC, allocate, then associate with an instance.
- Update DNS records to point to the Elastic IP, ensuring consistent access.

---

# Instance Roles
**Definition and Purpose:**
Instance roles, specifically IAM roles for EC2 instances, allow applications running on those instances to securely make API requests to AWS services without managing long-term credentials. The role is attached to the instance, and applications assume it to obtain temporary security credentials, enhancing security and simplifying credential management.
- An Instance role is like giving your EC2 instance a special permission card to access AWS services, like S3 or DynamoDB, without storing sensitive keys on it. It’s safer because AWS handles temporary credentials automatically, and you can control exactly what the instance can do.

### **Key Characteristics:**
- **Security Model:** Eliminates the need to distribute and manage access keys on instances, reducing risk of credential exposure. Temporary credentials are automatically rotated, managed by AWS Security Token Service (STS).
- **Permissions Management:** Define permissions via IAM policies in JSON format, similar to user policies, ensuring least privilege. Changes propagate to all instances using the role, with no maximum session duration for credentials ([Methods to assume a role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage-assume.html)).
- **Instance Profile:** EC2 uses an instance profile as a container for the IAM role, with one role per profile, but multiple profiles can include the same role. When using the console, the role and profile often have the same name, simplifying management.
- **Attachment Flexibility:** Can be specified at launch or attached/replaced on running or stopped instances, via console, CLI, or SDK.

### **How It Works:**
- You create an IAM role with specific permissions, attach it to your instance when launching or later, and applications on the instance use it to get temporary access. This means no need to manage access keys, and permissions update automatically if you change the role.

To use an Instance role:
1. Create an IAM role with trusted entities (e.g., EC2) and permissions (e.g., S3 read access).
2. Attach it to the instance at launch via the console (Advanced details > IAM instance profile) or CLI (`aws ec2 run-instances --iam-instance-profile Name=RoleName`).
3. For existing instances, modify via console (Actions > Security > Modify IAM role) or CLI (`aws ec2 associate-iam-instance-profile --iam-instance-profile Name=RoleName --instance-id i-1234567890abcdef0`).
4. Applications on the instance use AWS SDK or CLI, assuming the role via Instance Metadata Service, retrieving temporary credentials at `/identity-credentials/ec2/security-credentials/ec2-instance`.

### **Use Cases:**
- **Application Access to AWS Services:** Enable applications to interact with S3, DynamoDB, or other services without credentials, such as uploading files or querying databases.
- **Automation and CI/CD:** Scripts or tools like Jenkins on EC2 can assume roles for tasks like shutting down non-production instances, enhancing automation security.
- **Compliance and Security:** Ensure least privilege, monitored via AWS CloudTrail, aligning with regulations like GDPR and HIPAA.
- Only one role can be attached at a time, but it can be used by many instances.
- It follows the least privilege rule, so you grant only what’s needed.
- You can attach or replace it on existing instances using the AWS console or CLI.

### **Best Practices:**
- Follow least privilege, granting only necessary permissions, using IAM Access Analyzer to generate policies from CloudTrail logs ([IAM Access Analyzer policy generation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-policy-generation.html)).
- Regularly review and update roles, replacing instance profiles for permission changes to avoid delays (up to 1 hour for removal).
- Use instance identity roles for default access, supporting services like EC2 Instance Connect and Systems Manager, with ARN format `arn:aws:iam::account-number:assumed-role/aws:ec2-instance/instance-id`.

### **Practical Example:**
Create and attach an Instance role for S3 access:
- Open IAM console at [https://console.aws.amazon.com/iam/](https://console.aws.amazon.com/iam/).
- Create role, select EC2, attach policy (e.g., AmazonS3ReadOnlyAccess), name it "S3AccessRole".
- Launch EC2 instance, select "S3AccessRole" in Advanced details.
- On the instance, use AWS CLI: `aws s3 ls` to list buckets, leveraging the role's permissions.

---

### Comparative Table

| Feature                  | Elastic IP                          | Instance Role                          |
|--------------------------|-------------------------------------|----------------------------------------|
| **Purpose**              | Static public IP for consistent access | Secure AWS service access without credentials |
| **Key Benefit**          | Maintains IP during instance changes | Enhances security, automates credential management |
| **Association**          | With instance or network interface  | Attached to EC2 instance                |
| **Cost**                 | Charged if not associated with running instance | No additional cost, included in IAM     |
| **Limit**                | 5 per region by default             | One per instance, multiple instances per role |
| **Use Case**             | Web hosting, failover management    | Application access to S3, automation scripts |

---
