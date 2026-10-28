---
title: "Week 10 Worklog"
date: 2026-10-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

# Week 10 Worklog - CloudWatch Metrics and RAM Alerting

**Project:** Tomato leaf disease detection on AWS  
**Period:** Week 10 working days 1 to 5

### Week Objectives:
- Collect memory and disk metrics from the EC2 host.
- Build a focused resource monitoring view.
- Configure one RAM alarm with an SNS notification action.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | Review the difference between standard EC2 metrics and host metrics collected by the CloudWatch agent. Identify RAM and disk usage requirements. | W10 D1 | W10 D1 | CloudWatch agent guide |
| 2 | Install and configure the CloudWatch agent on EC2. Publish host memory and disk metrics under the PlantDisease/Host namespace. | W10 D2 | W10 D2 | CloudWatch agent guide |
| 3 | Inspect CPU, memory and disk graphs for the correct instance. Review metric dimensions, statistics and collection periods before building the dashboard. | W10 D3 | W10 D3 | CloudWatch agent guide |
| 4 | Configure one RAM alarm: Average memory usage above 85 percent for three consecutive one-minute periods. Review how missing data affects its state. | W10 D4 | W10 D4 | CloudWatch alarms guide |
| 5 | Prepare the SNS email notification action and subscription workflow. Review a controlled threshold test procedure and restoration to the operational threshold. | W10 D5 | W10 D5 | CloudWatch alarms guide |

### Achievements:
- Prepared a CloudWatch monitoring view for EC2 host resources.
- Defined one RAM alarm with an explicit threshold and evaluation window.
- Documented the notification setup and controlled alarm test procedure.
