---
title: "Week 8 Worklog"
date: 2026-10-16
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

# Week 08 Worklog - Connecting Amazon EC2 and Amazon S3

**Project:** Tomato leaf disease detection on AWS  
**Period:** Week 08 working days 1 to 5

### Week Objectives:
- Retrieve the model artifact set using the EC2 IAM role.
- Resolve credential and object permission issues.
- Verify real-model execution in the cloud environment.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | Confirm the instance profile is attached to EC2. Check the active AWS identity and Region without placing permanent access keys on the instance. | W08 D1 | W08 D1 | AWS IAM best practices |
| 2 | Download model.pth and class_to_idx.json from mobilenet-distilled-v1/. Use exact object keys and a dedicated checkpoint directory. | W08 D2 | W08 D2 | Amazon S3 user guide |
| 3 | Review missing credential and S3 access denied errors. Check the assumed role, object key and policy resource scope before changing permissions. | W08 D3 | W08 D3 | AWS IAM best practices |
| 4 | Calculate SHA256 hashes on EC2 and compare them with the source artifact manifest. Keep the checkpoint and label mapping paired. | W08 D4 | W08 D4 | Amazon S3 user guide |
| 5 | Load the verified model on EC2 and run a sample tomato leaf prediction. Review the returned class labels and record the model version. | W08 D5 | W08 D5 | Amazon EC2 getting started |

### Achievements:
- Connected EC2 to private S3 model storage through role-based access.
- Validated checkpoint integrity and the matching class mapping on EC2.
- Ran a sample prediction using the real model in the cloud environment.
