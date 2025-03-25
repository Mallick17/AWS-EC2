# EBS Encryption
EBS encryption is a security feature in AWS that ensures data stored on EBS volumes is encrypted at rest, protecting sensitive information from unauthorized access. It uses the Advanced Encryption Standard (AES) with 256-bit keys, managed through AWS Key Management Service (KMS), and is handled transparently on the servers hosting EC2 instances. This feature is crucial for compliance with regulations like GDPR, HIPAA, and PCI DSS, and for securing data in multi-tenant environments.

The attachment provided, "ec2-types.pdf," confirms that most EC2 instance types support EBS encryption, as indicated by a checkmark in the "EBS encryption" column, with some also supporting encryption in transit. This aligns with broader AWS documentation, which details the implementation and best practices for EBS encryption.

##### What is EBS Encryption?
EBS encryption ensures that data on EBS volumes is encrypted at rest using AES-256, a symmetric encryption algorithm formally approved by the US National Security Agency (NSA). The encryption process is managed by AWS, eliminating the need for users to build and maintain their own key management infrastructure. This is particularly beneficial for DevOps teams, as it simplifies security management while maintaining high performance.

The attachment highlights that EBS encryption is supported across most modern instance types, such as M5, C5, R5, and others, with specific tables indicating support with a "✔." This universal support ensures that nearly all current generation and previous generation instance types can leverage encryption, as noted in the "Security specifications" sections of the PDF.

##### How Does EBS Encryption Work?
EBS encryption uses AWS KMS keys for encrypting volumes and snapshots, with encryption operations occurring on the EC2 instance host servers. This ensures the security of both data-at-rest and data-in-transit between an instance and its attached EBS storage. The process is transparent, meaning applications and users interact with the volume as they would with an unencrypted one, with no additional action required.

Key details from the documentation include:
- **Encryption Method:** Uses symmetric encryption KMS keys; does not support asymmetric keys ([Amazon EBS encryption](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-encryption.html)).
- **KMS Key Specification:** You can specify a KMS key during volume creation; if not specified, it defaults to the source snapshot's key or the default KMS key if encryption by default is enabled.
- **Encryption by Default:** Automatically encrypts new volumes and snapshots with the default KMS key if enabled, a region-specific setting that cannot be disabled for individual volumes once set ([Enable Amazon EBS encryption by default](https://docs.aws.amazon.com/ebs/latest/userguide/encryption-by-default.html)).

When an instance interacts with an encrypted AMI, volume, or snapshot, a KMS key grant is issued to the instance's identity-only role, ensuring secure access without manual intervention ([Requirements for Amazon EBS encryption](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-encryption-requirements.html)).

##### Enabling EBS Encryption
There are two primary methods to enable EBS encryption:

1. **Per-Volume Encryption:** When creating an EBS volume, you can choose to encrypt it and select the KMS key to use. This can be done through the AWS Management Console, CLI, or SDK. For example, using the CLI, you can create an encrypted volume with: `aws ec2 create-volume --size 8 --availability-zone us-west-2a --encrypted --kms-key-id alias/aws/ebs`.

2. **Default Encryption:** You can configure your AWS account to enforce encryption for all new EBS volumes and snapshot copies in a specific region. To enable this:
   - Open the Amazon EC2 console, select the appropriate region.
   - Navigate to "EC2 Dashboard" > "Account Attributes" > "Data protection and security" > "EBS encryption" > "Manage."
   - Select "Enable," and choose either the AWS managed key (alias aws/ebs) or a symmetric customer managed encryption key. Click "Update EBS encryption."
   - This setting is region-specific and cannot be disabled for individual volumes once enabled ([Turn on automatic encryption of new Amazon EBS volumes and snapshot copies | AWS re:Post](https://repost.aws/knowledge-center/ebs-automatic-encryption)).

The attachment does not detail the enabling process but confirms support across instance types, reinforcing the importance of default encryption for comprehensive security.

##### Types of Encryption Keys
EBS encryption relies on AWS KMS for key management, with two types of keys available:
- **AWS Managed Keys:** These are created and managed by AWS, with the alias "aws/ebs" as the default for EBS encryption. They are suitable for basic use cases and are automatically rotated by AWS.
- **Customer Managed Keys (CMKs):** These are keys that you create and manage, offering more control over key rotation, deletion, and access policies. CMKs are recommended for specific compliance requirements, such as when sharing encrypted snapshots across accounts, as AWS managed keys cannot be shared ([New – Opt-in to Default Encryption for New EBS Volumes | AWS News Blog](https://aws.amazon.com/blogs/aws/new-opt-in-to-default-encryption-for-new-ebs-volumes/)).

The attachment does not specify key types but implies their use through the support for encryption, aligning with KMS documentation.


##### Best Practices for Managing Encrypted EBS Volumes
To maximize the security benefits of EBS encryption, consider the following best practices:
- **Enable Default Encryption:** Ensure all new volumes are encrypted by default to avoid accidental creation of unencrypted volumes, a region-specific setting that simplifies compliance ([Must-know best practices for Amazon EBS encryption | AWS Compute Blog](https://aws.amazon.com/blogs/compute/must-know-best-practices-for-amazon-ebs-encryption/)).
- **Use Customer Managed Keys:** For enhanced control, especially in regulated industries, use CMKs to manage key lifecycle and access policies, ensuring compliance with standards like GDPR and HIPAA.
- **Regular Key Rotation:** Enable automatic key rotation in KMS to enhance security, with existing volumes retaining access to previous keys unless deleted ([Amazon EBS Encryption | Sysdig](https://sysdig.com/learn-cloud-native/amazon-ebs-encryption/)).
- **Monitor Access:** Use AWS CloudTrail to monitor access to encryption keys, detecting unauthorized attempts and ensuring audit trails for compliance.
- **Handle Existing Unencrypted Volumes:** For unencrypted volumes, create snapshots and generate encrypted copies, ensuring no downtime for critical infrastructure during remediation ([Automatically encrypt existing and new Amazon EBS volumes | AWS Prescriptive Guidance](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/automatically-encrypt-existing-and-new-amazon-ebs-volumes.html)).

The attachment does not cover best practices but supports the need for encryption through instance type specifications, aligning with these recommendations.

##### Specific Use Cases and Scenarios
EBS encryption is particularly important in scenarios where data security is paramount:
- **Sensitive Data Storage:** For storing personally identifiable information (PII), financial records, or healthcare data, ensuring compliance with regulations like GDPR, HIPAA, or PCI DSS ([EBS Encryption | CoreStack](https://www.corestack.io/aws-security-best-practices/ebs-encryption/)).
- **Intellectual Property Protection:** Securing proprietary data or confidential business information to prevent unauthorized access.
- **Multi-Tenant Environments:** Ensuring data isolation and security in shared environments, such as in cloud-based SaaS applications.

The attachment's focus on security specifications, including EBS encryption, underscores its role in these use cases, particularly for modern instance types.

#### Tables for Clarity
Below is a table summarizing key aspects of EBS encryption:

| Aspect                  | Details                                                                 |
|------------------------|-------------------------------------------------------------------------|
| **Encryption Method**   | AES-256, managed by AWS KMS, symmetric keys only.                      |
| **Enabling Options**    | Per-volume or default encryption per region.                           |
| **Key Types**           | AWS managed keys (default) or customer managed keys (CMKs).            |
| **Performance Impact**  | Minimal, no effect on IOPS, slight latency increase, handled in hardware. |
| **Best Practices**      | Enable default, use CMKs, rotate keys, monitor access.                 |
| **Use Cases**           | Sensitive data, compliance, multi-tenant environments.                 |

And a comparison of key types:

| Key Type               | Description                                      | Use Case                                    |
|------------------------|--------------------------------------------------|---------------------------------------------|
| AWS Managed Keys       | Created and managed by AWS, default for EBS.     | Basic security, no sharing across accounts. |
| Customer Managed Keys  | Created and managed by user, more control.       | Compliance, sharing snapshots across accounts. |
