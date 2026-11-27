---
title: "Proposal"
date: 2026-10-16
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# AWS Plant Disease Detection Project Proposal

## Twelve week implementation and validation plan

16 October 2026  |  Sydney  |  ap-southeast-2

We propose a twelve-week implementation and validation program for a containerized tomato leaf classification application on EC2, S3 and CloudWatch. The plan starts from the existing prototype and closes deployment, evidence and operational gaps.


### 1 Executive summary

The project will deliver a reproducible AWS demonstration with a responsive frontend, CPU inference API, privately stored model artifacts and basic CloudWatch observability. GitHub Actions will publish images; a controlled manual procedure will deploy and roll back compatible image and model versions. This scope fits a backend and MLOps learning project without requiring serverless or microservices claims.

The project budget follows the low-traffic AWS demonstration configuration in the project overview: **USD 50 per month before tax**, or **USD 150 for three monthly billing periods** covering the twelve-week project. With USD 200 in eligible AWS credits, four months of this configuration are covered. Credit availability and expiration depend on the account. [6–12]


### 2 Problem statement

A locally trained classifier is difficult to reproduce on another host when model weights, class ordering, preprocessing and dependencies are managed separately. LAN-only access limits demonstrations, and an apparently healthy process may still fail prediction because of missing weights, CORS, queue saturation or permission errors. Source-code synchronization alone does not update a running container.

The project therefore needs a versioned release identity, least-privilege access to artifacts, evidence of real inference on AWS, actionable host monitoring and a documented recovery process. The current prototype supplies working components, but it does not yet establish high availability, field accuracy or automated production deployment.


### 3 Solution architecture


#### AWS services used

| Service | Role | Design choice |
| --- | --- | --- |
| Amazon EC2 | Run frontend and API containers | One x86_64 host in Sydney |
| Amazon S3 | Store checkpoint and class map | Private versioned artifacts |
| Amazon CloudWatch | Container logs, host metrics, and dashboard | 60 second Agent metrics; 7 day logs |
| IAM and VPC | Identity and network controls | Instance role and source IP rules |


#### Component design

The browser reaches Next.js on port 3000 and FastAPI on port 8000 at the same EC2 hostname. FastAPI loads a MobileNet checkpoint once per process and validates the supplied class map. Prediction and Grad-CAM++ share a bounded execution slot. S3 is accessed at deployment time; uploaded user images do not need to be persisted in S3 for the current use case.

```text
GitHub Actions -> GHCR -> manual deployment to EC2
Private S3 artifacts -> host version directory -> read-only API mount
Browser -> Next.js frontend and FastAPI API
Container logs + Agent metrics -> CloudWatch dashboard
```

The architecture deliberately uses a public subnet with restricted inbound access for the demonstration. S3 VPC endpoints, on-premises connectivity, DNS simulation, load balancing and autoscaling are outside the baseline. A private-network extension can be evaluated later without being reported as an implemented feature.


### 4 Technical implementation

Build the frontend with its existing multi-stage Dockerfile and standalone Next.js output. Build the API with Dockerfile.deploy so the checkpoint remains outside the image. CI gates include Python tests, frontend build, backend build and frontend container smoke tests. Publish both images with full commit tags and record digests; tests using fixture weights remain distinct from real-model acceptance tests.

Create the EC2 instance profile with scoped GetObject permission and CloudWatch delivery permissions. Download model artifacts from mobilenet-distilled-v1 and compare SHA256 values before mounting them read-only. Bind the API and frontend ports, configure CORS_ORIGINS with the exact page origin and use restart unless-stopped. Recreate containers when changing image or environment settings.

Validate readiness, real-image prediction, response schema and model identity. Then validate browser upload and Grad-CAM++ on desktop and phone. Measure API latency under a declared request count and concurrency instead of interpreting a single request as p95. Evaluate macro F1 and per-class recall only when an independent labeled dataset is available; do not promise a diagnostic accuracy without that evaluation.

Use CloudWatch Logs for both containers and CloudWatch Agent metrics for host memory and disk, with seven-day log retention. Review the host dashboard during operation. Record rollback and EC2 stop/start rehearsals, including public IP and CORS updates.


### 5 Timelines and milestones

Each week receives 20 planned working hours, totaling 240 hours. The proposed schedule runs from September 7 through November 27, 2026, with November 27 as the final working day, ahead of the November 30 cutoff. This is a planned schedule, not a reconstruction of dates already worked. AWS operating costs follow the monthly configuration in the budget below; they are not calculated from the planned working hours.

| Week and target dates | Work package | Milestone evidence |
| --- | --- | --- |
| 1 · Sep 7–11 | Scope and acceptance criteria | Architecture scope agreed |
| 2 · Sep 14–18 | Audit model preprocessing and labels | Artifact manifest and hashes |
| 3 · Sep 21–25 | Harden API validation and errors | API contract and tests |
| 4 · Sep 28–Oct 2 | Validate frontend and explanations | Browser workflow verified locally |
| 5 · Oct 5–9 | Package demo containers | Two images pass smoke tests |
| 6 · Oct 12–16 | Complete CI and GHCR delivery | Published release digests |
| 7 · Oct 19–23 | Prepare AWS IAM network and budget | EC2 role and restricted connectivity |
| 8 · Oct 26–30 | Store and retrieve S3 artifacts | Download hashes match |
| 9 · Nov 2–6 | Deploy EC2 and test real inference | Desktop and phone acceptance |
| 10 · Nov 9–13 | Configure CloudWatch observability | Log groups, host metrics, and dashboard checked |
| 11 · Nov 16–20 | Rehearse rollback and measure latency | Recovery and performance evidence |
| 12 · Nov 23–27 | Finalize report demo and cleanup | Handover and cost review |


### 6 Cost

The approved low-traffic demonstration budget for Sydney (`ap-southeast-2`) is **USD 50 per month before tax**. It covers one `t3.medium` EC2 host running 730 hours per month, a 20 GiB gp3 volume, one public IPv4 address, small S3 model artifacts, and low-volume monitoring. The monthly allocation is:

| Service / usage | Monthly cost |
|---|---:|
| EC2 Linux `t3.medium`, 730 hours | $38.54 |
| EBS gp3, 20 GiB | $1.60 |
| Public IPv4, 730 hours | $3.65 |
| S3 model checkpoint and class map | $0.01 |
| CloudWatch logs, host metrics, and dashboard | $0.00 |
| Internet data transfer within the 100 GB/month shared allowance | $0.00 |
| Operating headroom | $6.20 |
| **Monthly total** | **$50.00** |

The twelve-week project reserves three monthly billing periods, for a total project budget of **USD 150 before tax**. Eligible AWS credits of USD 200 cover four months at this monthly budget, leaving USD 50 for the twelve-week period. Credit eligibility, available balance, and expiration depend on the AWS account; confirm them in the Billing Console. Configure a monthly budget alert and review gross usage regularly, since budget notifications do not stop resources automatically. [6–12]

This budget covers a single host and low-traffic demo use. It excludes tax, GPU training, load balancing, NAT Gateway, databases, high availability, traffic beyond the stated allowance, excess logs or metrics, and T3 CPU-credit surcharges. Training and research runs use a local machine; GHCR and GitHub Actions remain within included account usage. Stopping EC2 reduces compute charges, while EBS storage remains billable. Check the AWS Pricing Calculator if the region or configuration changes. [6–10]


### 7 Risk assessment

| Risk | Likelihood and impact | Mitigation and owner |
| --- | --- | --- |
| Credit depletion | Medium / High | Operator stops EC2 and reviews gross costs weekly |
| Instance capacity or plan restriction | Medium / Medium | Operator checks eligible x86_64 type and revises budget |
| Incorrect model or class order | Medium / High | ML developer verifies hashes map and preprocessing |
| CORS or public IP change | High / Medium | Backend operator recreates container with correct origin |
| Access or credential exposure | Medium / High | Operator uses instance role private S3 and restricted SSH |
| Host outage or saturation | Medium / High | Backend operator measures queue load and rehearses recovery |
| Field data differs from training | High / High | ML developer collects labeled samples and limits claims |

The initial demonstrator accepts single-host downtime and HTTP access restricted to approved client IPs. Public production use requires a separate security and availability review, including HTTPS and authentication. CloudWatch logs and host metrics provide operational visibility but do not establish service availability or ML quality.


### 8 Expected outcomes

| Outcome | Acceptance evidence |
| --- | --- |
| Reproducible release | Two image digests plus checkpoint map hashes and config |
| Working AWS application | Health and prediction succeed; browser prediction and explanation verified |
| Secure artifact retrieval | Private S3 GetObject by instance role with matching hashes |
| Operational visibility | Container logs, host metrics, and dashboard reviewed |
| Documented recovery | Rollback and EC2 stop/start rehearsal with observed timings |
| Measured performance | Latency report states image hardware concurrency samples and p95 |
| Controlled cost | Weekly gross-cost log and final cleanup evidence |

Success means a demonstrable and documented deployment within the approved spending envelope. It does not mean a proven agricultural diagnosis service, a zero-downtime platform or a completed twelve-week historical project. Accuracy and recovery objectives will be reported only after measurement.


### References

AWS documentation and pricing references accessed on 16 October 2026. Usage allowances in the proposal are planning assumptions, not a regional price quotation.

[1] Project repository and release history

[2] EC2 stop and start behavior

[3] S3 HeadObject permissions and error responses

[4] CloudWatch Agent installation

[5] CloudWatch operational monitoring

[6] EC2 On Demand pricing

[7] EBS pricing

[8] Public IPv4 pricing

[9] CloudWatch pricing

[10] S3 pricing

[11] AWS credits

[12] AWS Budgets
