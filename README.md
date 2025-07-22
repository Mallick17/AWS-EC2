# AWS Instance Metadata Service v2 (IMDSv2)

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
