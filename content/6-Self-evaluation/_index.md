---
title: "Self-Assessment"
date: 2026-10-16
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

# Self Assessment


### 1 Skills demonstrated

The work demonstrates separation of image delivery from deployment, artifact integrity verification, IAM role use, Docker runtime configuration and CloudWatch integration. Actual issues were resolved across Windows SSH key ACLs, missing instance credentials, incorrect S3 prefix or object access, and missing log-read permission. These are backend operations skills rather than evidence of a fully managed production platform.


### 2 Troubleshooting lessons

| Symptom | Cause and resolution |
| --- | --- |
| UNPROTECTED PRIVATE KEY FILE | Restrict the Windows key ACL; use the matching EC2 key pair |
| Unable to locate credentials | Attach EC2 instance role; verify STS identity |
| 403 HeadObject | Check exact S3 key and GetObject resource prefix |
| AccessDenied FilterLogEvents | Add scoped permission to read application logs |
| Page opens but prediction fails | Check API port and exact CORS_ORIGINS; recreate container after changes |


### 3 Limitations and next validation

A single EC2 instance is a single point of failure. The HTTP demo has no application authentication or load balancer. The RAM alarm does not establish endpoint availability or model accuracy. Inference and explanation share one execution slot per backend process, so concurrent demand can produce service_busy or timeout responses. The 29.23 ms sample is not a throughput or percentile measurement.

The next acceptance checks are browser prediction and explanation on AWS, confirmed SNS notification and restoration to the 85 percent threshold, a documented rollback rehearsal and a repeatable latency test. Field accuracy requires an independent labeled dataset; no accuracy percentage is claimed by this workshop.


### 4 Internship Evaluation Criteria

| No. | Criteria | Description | Good | Fair | Average |
| --- | --- | --- | --- | --- | --- |
| 1 | **Professional knowledge & skills** | Understanding of the field, applying knowledge in practice, proficiency with tools, work quality | ✅ | ☐ | ☐ |
| 2 | **Ability to learn** | Ability to absorb new knowledge and learn quickly | ✅ | ☐ | ☐ |
| 3 | **Proactiveness** | Taking initiative, seeking out tasks without waiting for instructions | ✅ | ☐ | ☐ |
| 4 | **Sense of responsibility** | Completing tasks on time and ensuring quality | ✅ | ☐ | ☐ |
| 5 | **Discipline** | Adhering to schedules, rules, and work processes | ✅ | ☐ | ☐ |
| 6 | **Progressive mindset** | Willingness to receive feedback and improve oneself | ✅ | ☐ | ☐ |
| 7 | **Communication** | Presenting ideas and reporting work clearly | ✅ | ☐ | ☐ |
| 8 | **Teamwork** | Working effectively with colleagues and participating in teams | ✅ | ☐ | ☐ |
| 9 | **Professional conduct** | Respecting colleagues, partners, and the work environment | ✅ | ☐ | ☐ |
| 10 | **Problem-solving skills** | Identifying problems, proposing solutions, and showing creativity | ✅ | ☐ | ☐ |
| 11 | **Contribution to project/team** | Work effectiveness, innovative ideas, recognition from the team | ✅ | ☐ | ☐ |
| 12 | **Overall** | General evaluation of the entire internship period | ✅ | ☐ | ☐ |
