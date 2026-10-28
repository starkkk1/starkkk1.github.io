---
title: "Week 2 Worklog"
date: 2026-10-16
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

# Week 02 Worklog - Model Training and Artifact Preparation

**Project:** Tomato leaf disease detection on AWS  
**Period:** Week 02 working days 1 to 5

### Week Objectives:
- Train and review a lightweight tomato leaf classifier.
- Prepare model files suitable for CPU inference on Amazon EC2.
- Record model identity without relying on unsupported accuracy claims.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | Review the MobileNetV3 Small configuration and initialize the training environment. Check that model output dimensions match the class index mapping. | W02 D1 | W02 D1 | Project dataset and model notes |
| 2 | Run an initial training experiment. Review training and validation loss to identify configuration errors and signs of overfitting. | W02 D2 | W02 D2 | Project dataset and model notes |
| 3 | Review the lightweight student model and knowledge distillation workflow. Compare candidate checkpoints using the same validation partition and preprocessing. | W02 D3 | W02 D3 | Project dataset and model notes |
| 4 | Evaluate the selected checkpoint on held-out images. Inspect class-level errors and verify CPU inference with the matching class mapping. | W02 D4 | W02 D4 | Project dataset and model notes |
| 5 | Export the selected checkpoint as model.pth and the label mapping as class_to_idx.json. Calculate SHA256 hashes for later cloud integrity checks. | W02 D5 | W02 D5 | Project dataset and model notes |

### Achievements:
- Prepared a lightweight model checkpoint and compatible class mapping.
- Reviewed model behavior and CPU inference before cloud deployment.
- Created identifiable artifacts for storage and retrieval on Amazon S3.
