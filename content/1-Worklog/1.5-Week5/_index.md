---
title: "Week 5 Worklog"
date: 2026-10-16
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

# Week 05 Worklog - Amazon S3 Model Artifact Storage

**Project:** Tomato leaf disease detection on AWS  
**Period:** Week 05 working days 1 to 5

### Week Objectives:
- Store the selected model and label mapping in a private S3 bucket.
- Use a consistent object prefix and verify artifact integrity.
- Review storage permissions and model release organization.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | Create the project S3 bucket in the selected Region. Review Block Public Access and encryption settings before uploading any model artifacts. | W05 D1 | W05 D1 | Amazon S3 user guide |
| 2 | Organize the model release under mobilenet-distilled-v1/. Keep the checkpoint and class mapping together as one compatible artifact set. | W05 D2 | W05 D2 | Amazon S3 user guide |
| 3 | Upload model.pth and class_to_idx.json. Review object names, sizes and metadata to ensure that the intended files were stored. | W05 D3 | W05 D3 | Amazon S3 user guide |
| 4 | Compare the stored artifact names with the intended IAM resource prefix. Review how an incorrect key or insufficient permissions can cause access failures. | W05 D4 | W05 D4 | AWS IAM best practices |
| 5 | Download the artifact set for an integrity check and compare SHA256 hashes. Review versioning as an option for retaining earlier model artifacts. | W05 D5 | W05 D5 | Amazon S3 user guide |

### Achievements:
- Organized the model artifacts in private Amazon S3 storage.
- Established a consistent prefix for the selected model release.
- Prepared a checksum-based procedure for validating downloaded artifacts.
