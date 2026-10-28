---
title: "Week 9 Worklog"
date: 2026-10-16
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

# Week 09 Worklog - Amazon CloudWatch Logs

**Project:** Tomato leaf disease detection on AWS  
**Period:** Week 09 working days 1 to 5

### Week Objectives:
- Collect operational logs from the EC2 workload.
- Organize log groups and review retention settings.
- Troubleshoot access to cloud logs.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | Review CloudWatch log groups, log streams and log events. Identify which model initialization and inference events are useful for troubleshooting. | W09 D1 | W09 D1 | CloudWatch Logs guide |
| 2 | Configure the workload to send operational output to CloudWatch Logs. Use identifiable log groups and streams for the project environment. | W09 D2 | W09 D2 | CloudWatch Logs guide |
| 3 | Review log delivery permissions on the EC2 role. Distinguish successful log ingestion from permission to search or read the stored events. | W09 D3 | W09 D3 | AWS IAM best practices |
| 4 | Inspect initialization messages and sample inference logs. Use time ranges and filters to locate failures without exposing credentials or private images. | W09 D4 | W09 D4 | CloudWatch Logs guide |
| 5 | Review the seven-day retention target and log storage costs. Document the scoped logs:FilterLogEvents permission required for command-line log searches. | W09 D5 | W09 D5 | CloudWatch Logs guide |

### Achievements:
- Organized operational logs in CloudWatch for the EC2 workload.
- Prepared a repeatable approach to searching cloud log events.
- Reviewed log retention and the permissions required to read logs.
