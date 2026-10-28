---
title: "Prerequisites & Setup"
date: 2026-10-28T10:00:00+07:00
weight: 20
chapter: false
pre: "<b>5.2. </b>"
---

# Prerequisites & Setup

Before launching the Plant Disease Detection application on Amazon Web Services, verify the following account permissions, compute infrastructure, and security settings.

---

### Prerequisites & Dependency Matrix

The table below summarizes the exact configuration requirements recorded during the deployment:

| Dependency Area | Required Specification | Purpose & Details |
|---|---|---|
| **AWS Account** | Personal AWS Account | Dedicated sandbox environment for demonstration and cost control |
| **AWS Region** | Sydney (`ap-southeast-2`) | Low network latency and full regional availability of EC2, S3, and CloudWatch |
| **Compute Engine** | Amazon EC2 (Amazon Linux 2023 x86_64) | Production host running Docker Engine and AWS CLI v2 |
| **VPC & Network** | Public Subnet (`0.0.0.0/0` -> Internet Gateway) | Direct routing for package updates and inbound web access; Public IPv4 assigned |
| **Inbound Security** | Port `22` (SSH), Port `3000` (Frontend), Port `8000` (API) | Restricted strictly to the operator's approved public IPv4 client address (`/32`) |
| **Model Storage** | Amazon S3 (Private Bucket) | Stores model checkpoints under the `mobilenet-distilled-v1/` prefix |
| **IAM Instance Role** | EC2 Instance Profile | Grants `s3:GetObject` on model artifacts and `CloudWatchAgentServerPolicy` |
| **Log Ingestion Role** | `logs:FilterLogEvents` | Required for operators to tail and query CloudWatch container logs via the AWS CLI |
| **Model Artifacts** | Verified `model.pth` and `class_to_idx.json` | Release artifacts matched via cryptographic SHA256 checksums |

> [!WARNING]
> Do NOT use wildcard `0.0.0.0/0` for inbound SSH (port 22) or API management ports. Restrict ingress rules to your specific public IP address to prevent unauthorized intrusion.

---

### Step 1: AWS CLI Configuration & Identity Verification

Ensure your operator machine has **AWS CLI v2** installed and configured with credentials that have sufficient administrative permissions:

![AWS CLI Configuration](/images/5-Workshop/5.2-Prerequisite/aws_cli_configure.png?width=700px)

Run the following command to verify your authenticated caller identity and set the active region to Sydney:

```bash
# Verify AWS identity
aws sts get-caller-identity

# Set default region to ap-southeast-2
export AWS_DEFAULT_REGION="ap-southeast-2"
aws configure set default.region ap-southeast-2
```

---

### Step 2: EC2 Security Group Inbound Rules

Create a dedicated Security Group named `plant-disease-sg` in your default VPC. Add the following inbound rules:

| Type | Protocol | Port Range | Source | Description |
|---|---|---|---|---|
| SSH | TCP | `22` | `<YOUR_CLIENT_IP>/32` | Administrative SSH shell access |
| Custom TCP | TCP | `3000` | `<YOUR_CLIENT_IP>/32` | Next.js Frontend Web Application |
| Custom TCP | TCP | `8000` | `<YOUR_CLIENT_IP>/32` | FastAPI CPU Inference REST API |

---

### Step 3: EC2 Instance Profile & IAM Permissions

Attach an IAM Role to your EC2 instance so that containers and AWS CLI scripts can securely access Amazon S3 and publish CloudWatch metrics **without storing long-lived AWS Access Keys or Secret Keys** on the server.

Attach the following IAM policies to the role:
1. **AmazonCloudWatchAgentServerPolicy** (AWS Managed Policy): Allows the CloudWatch Agent to emit custom host metrics and create log streams.
2. **PlantDiseaseS3Policy** (Inline Policy): Grants read-only access strictly to the model artifacts prefix:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3ModelArtifactsDownload",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::plant-disease-model-artifacts-ap-southeast-2",
        "arn:aws:s3:::plant-disease-model-artifacts-ap-southeast-2/mobilenet-distilled-v1/*"
      ]
    },
    {
      "Sid": "AllowOperatorLogInspection",
      "Effect": "Allow",
      "Action": [
        "logs:FilterLogEvents",
        "logs:GetLogEvents",
        "logs:DescribeLogStreams"
      ],
      "Resource": "arn:aws:logs:ap-southeast-2:*:log-group:/plant-disease/*"
    }
  ]
}
```

![IAM Role Trust Policy](/images/5-Workshop/5.4-Deployment-strategy/iam_audit_role_trust_policy.png?width=700px)

---

### Step 4: Provisioning EC2 Host Software

Once logged into your EC2 instance via SSH, install Docker Engine and the CloudWatch Agent:

```bash
# Update Amazon Linux 2023 packages
sudo dnf update -y

# Install Docker and Git
sudo dnf install -y docker git
sudo systemctl enable --now docker
sudo usermod -aG docker ec2-user

# Install CloudWatch Agent
sudo dnf install -y amazon-cloudwatch-agent

# Verify installation
docker --version
aws --version
```
