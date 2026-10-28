---
title: "Week 6 Worklog"
date: 2026-10-16
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

# Week 06 Worklog - Amazon VPC and Network Access

**Project:** Tomato leaf disease detection on AWS  
**Period:** Week 06 working days 1 to 5

### Week Objectives:
- Understand the network path between the Internet, EC2 and S3.
- Select an appropriate subnet and review its routing.
- Restrict inbound access with Security Groups.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | Review VPC, subnet, route table and Internet Gateway concepts. Draw the network path required to reach the project EC2 instance. | W06 D1 | W06 D1 | Amazon VPC user guide |
| 2 | Inspect the available VPC and subnet configuration. Reuse a suitable public subnet after reviewing its route table and public IPv4 settings. | W06 D2 | W06 D2 | Amazon VPC user guide |
| 3 | Review outbound connectivity for retrieving model artifacts from S3. Compare the existing Internet route with the optional S3 gateway endpoint approach. | W06 D3 | W06 D3 | Amazon S3 user guide |
| 4 | Prepare Security Group rules for administrative access. Restrict SSH port 22 to the operator public IP and distinguish inbound from outbound rules. | W06 D4 | W06 D4 | Amazon EC2 getting started |
| 5 | Review the planned web access rules and basic timeout troubleshooting. Document the relationship between the instance address, route table and Security Group. | W06 D5 | W06 D5 | Amazon VPC user guide |

### Achievements:
- Mapped the VPC routing required by the cloud demonstration.
- Selected a suitable subnet and defined restricted SSH access.
- Documented a network troubleshooting sequence and optional S3 endpoint improvement.
