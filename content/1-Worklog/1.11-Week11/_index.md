---
title: "Week 11 Worklog"
date: 2026-10-16
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

# Week 11 Worklog - Secure Public Access to the EC2 Workload

**Project:** Tomato leaf disease detection on AWS  
**Period:** Week 11 working days 1 to 5

### Week Objectives:
- Connect the existing domain to the EC2 public address.
- Enable HTTPS access while retaining domain protection.
- Reduce unnecessary public exposure through Security Groups.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | Review the DNS path for caycachua.id.vn and identify where its records are managed. Distinguish domain registration from authoritative DNS hosting. | W11 D1 | W11 D1 | iNET SSL and TLS guide |
| 2 | Point the root A record to the current EC2 public IPv4. Review why protected DNS can return proxy addresses instead of the origin address. | W11 D2 | W11 D2 | iNET SSL and TLS guide |
| 3 | Review HTTP and HTTPS access requirements on EC2. Permit ports 80 and 443 while keeping SSH restricted to the operator address. | W11 D3 | W11 D3 | Amazon EC2 getting started |
| 4 | Review the domain certificate and HTTPS connection to EC2. Distinguish the visitor certificate from the origin certificate and the Full Strict setting. | W11 D4 | W11 D4 | iNET SSL and TLS guide |
| 5 | Remove unnecessary public application port rules after successful domain access. Verify that the model demonstration remains reachable through the protected domain. | W11 D5 | W11 D5 | Amazon EC2 getting started |

### Achievements:
- Connected the project domain to the EC2-hosted demonstration.
- Established HTTPS access and reviewed certificate validation at the origin.
- Reduced public exposure by removing unnecessary application port rules.
