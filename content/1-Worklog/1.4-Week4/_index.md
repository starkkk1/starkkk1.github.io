---
title: "Week 4 Worklog"
date: 2026-10-16
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

# Week 04 Worklog - AWS Identity and Access Management

**Project:** Tomato leaf disease detection on AWS  
**Period:** Week 04 working days 1 to 5

### Week Objectives:
- Understand users, roles, policies and temporary credentials.
- Prepare EC2 access to the model artifacts without embedding access keys.
- Apply least privilege to storage and monitoring permissions.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | Review IAM identities, policy actions and resource ARNs. Separate the human operator permissions from permissions required by the EC2 workload. | W04 D1 | W04 D1 | AWS IAM best practices |
| 2 | Review root account protection and multi-factor authentication. Define a routine access approach that avoids using root credentials for project operations. | W04 D2 | W04 D2 | AWS IAM best practices |
| 3 | Prepare an EC2 service role and instance profile. Review the trust policy that permits EC2 to obtain temporary AWS credentials. | W04 D3 | W04 D3 | AWS IAM best practices |
| 4 | Scope S3 read permissions to the required model artifact prefix. Separate permission to upload artifacts from permission to retrieve them on EC2. | W04 D4 | W04 D4 | AWS IAM best practices |
| 5 | Review CloudWatch permissions for publishing metrics, sending logs and reading logs. Document why logs:FilterLogEvents is distinct from log delivery permissions. | W04 D5 | W04 D5 | CloudWatch Logs guide |

### Achievements:
- Prepared an IAM role design for EC2 access to AWS resources.
- Defined restricted S3 artifact access and monitoring permissions.
- Clarified the difference between workload credentials and operator access.
