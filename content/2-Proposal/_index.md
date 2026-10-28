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

The project will deliver a reproducible AWS demonstration with a responsive frontend, CPU inference API, privately stored model artifacts and one operational RAM alarm. GitHub Actions will publish images; a controlled manual procedure will deploy and roll back compatible image and model versions. This scope fits a backend and MLOps learning project without requiring serverless or microservices claims.

The planning scenario uses 240 EC2 hours over twelve weeks and a gross budget of USD 67.20 including a 20 percent contingency. The account screenshot showed USD 100 available; an additional USD 100 is not assumed to be earned. Actual credit eligibility, expiry, regional prices and tax must be checked before spending. The estimate is a planning allowance, not an AWS quotation. [6–12]


### 2 Problem statement

A locally trained classifier is difficult to reproduce on another host when model weights, class ordering, preprocessing and dependencies are managed separately. LAN-only access limits demonstrations, and an apparently healthy process may still fail prediction because of missing weights, CORS, queue saturation or permission errors. Source-code synchronization alone does not update a running container.

The project therefore needs a versioned release identity, least-privilege access to artifacts, evidence of real inference on AWS, actionable host monitoring and a documented recovery process. The current prototype supplies working components, but it does not yet establish high availability, field accuracy or automated production deployment.


### 3 Solution architecture


#### AWS services used

| Service | Role | Design choice |
| --- | --- | --- |
| Amazon EC2 | Run frontend and API containers | One x86_64 host in Sydney |
| Amazon S3 | Store checkpoint and class map | Private versioned artifacts |
| Amazon CloudWatch | Logs metrics dashboard and RAM alarm | 60 second Agent metrics; 7 day logs |
| IAM and VPC | Identity and network controls | Instance role and source IP rules |
| Amazon SNS | Deliver alarm email | One confirmed email subscription |


#### Component design

The browser reaches Next.js on port 3000 and FastAPI on port 8000 at the same EC2 hostname. FastAPI loads a MobileNet checkpoint once per process and validates the supplied class map. Prediction and Grad-CAM++ share a bounded execution slot. S3 is accessed at deployment time; uploaded user images do not need to be persisted in S3 for the current use case.

```text
GitHub Actions -> GHCR -> manual deployment to EC2
Private S3 artifacts -> host version directory -> read-only API mount
Browser -> Next.js frontend and FastAPI API
Container logs + Agent metrics -> CloudWatch -> RAM alarm -> SNS
```

The architecture deliberately uses a public subnet with restricted inbound access for the demonstration. S3 VPC endpoints, on-premises connectivity, DNS simulation, load balancing and autoscaling are outside the baseline. A private-network extension can be evaluated later without being reported as an implemented feature.


### 4 Technical implementation

Build the frontend with its existing multi-stage Dockerfile and standalone Next.js output. Build the API with Dockerfile.deploy so the checkpoint remains outside the image. CI gates include Python tests, frontend build, backend build and frontend container smoke tests. Publish both images with full commit tags and record digests; tests using fixture weights remain distinct from real-model acceptance tests.

Create the EC2 instance profile with scoped GetObject permission and CloudWatch delivery permissions. Download model artifacts from mobilenet-distilled-v1 and compare SHA256 values before mounting them read-only. Bind the API and frontend ports, configure CORS_ORIGINS with the exact page origin and use restart unless-stopped. Recreate containers when changing image or environment settings.

Validate readiness, real-image prediction, response schema and model identity. Then validate browser upload and Grad-CAM++ on desktop and phone. Measure API latency under a declared request count and concurrency instead of interpreting a single request as p95. Evaluate macro F1 and per-class recall only when an independent labeled dataset is available; do not promise a diagnostic accuracy without that evaluation.

Use CloudWatch Logs for both containers and Agent metrics for RAM and disk. Retain logs for seven days and configure one RAM alarm at Average greater than 85 percent for three one-minute datapoints. Confirm SNS delivery with a controlled threshold test, then restore 85. Record rollback and EC2 stop/start rehearsals, including public IP and CORS updates.


### 5 Timelines and milestones

Each week receives 20 planned working hours, totaling 240 hours. This is a proposed twelve-week schedule, not a reconstruction of dates already worked. EC2 runtime is separately capped at a planning total of 240 hours; it need not match every working hour.

| Week | Work package | Milestone evidence |
| --- | --- | --- |
| 1 20 h | Scope and acceptance criteria | Architecture scope agreed |
| 2 20 h | Audit model preprocessing and labels | Artifact manifest and hashes |
| 3 20 h | Harden API validation and errors | API contract and tests |
| 4 20 h | Validate frontend and explanations | Browser workflow verified locally |
| 5 20 h | Package production containers | Two images pass smoke tests |
| 6 20 h | Complete CI and GHCR delivery | Published release digests |
| 7 20 h | Prepare AWS IAM network and budget | EC2 role and restricted connectivity |
| 8 20 h | Store and retrieve S3 artifacts | Download hashes match |
| 9 20 h | Deploy EC2 and test real inference | Desktop and phone acceptance |
| 10 20 h | Configure CloudWatch and RAM alarm | Metrics logs and email test |
| 11 20 h | Rehearse rollback and measure latency | Recovery and performance evidence |
| 12 20 h | Finalize report demo and cleanup | Handover and cost review |


### 6 Budget estimation

Budget in USD before tax, credits and discounts, for Sydney. Assume one c7i-flex.large or another approved x86_64 instance only after checking availability and price. The existing instance type has not been verified. The EC2 allowance of USD 0.12 per hour and gp3 allowance of USD 0.12 per GiB-month are deliberately rounded planning inputs, not claimed Sydney list prices. Recalculate if the selected type exceeds the allowance. Public IPv4 uses the published USD 0.005 per hour rate. [6–8]

| Item | Basis | USD |
| --- | --- | --- |
| EC2 | 240 h × $0.12 | $28.80 |
| Public IPv4 | 240 h × $0.005 | $1.20 |
| EBS gp3 | 30 GiB × 3 months × $0.12 | $10.80 |
| CloudWatch metrics dashboard alarm | $3/month allowance × 3 | $9.00 |
| CloudWatch Logs and queries | $1/month allowance × 3 | $3.00 |
| S3 storage versions requests | Project allowance | $1.00 |
| SNS email | Project allowance | $0.20 |
| Data transfer | Low-traffic allowance | $2.00 |
| Subtotal |  | $56.00 |
| Contingency | 20% × $56.00 | $11.20 |
| Total |  | $67.20 |

The twelve weeks span 84 days; storage and fixed monitoring allowances are rounded up to three months. CloudWatch and S3 amounts are spending envelopes, not exact unit-rate calculations, and do not rely on Free Tier exemptions. The transfer allowance is suitable only for a small demonstration. Taxes, domain purchase, load balancers, NAT Gateway, GPUs, model training and extra VPC endpoints are excluded. [7–10]

At the same assumptions, leaving EC2 and its public IPv4 running for all 2,016 hours of twelve weeks raises the subtotal to USD 278.00 and the total with contingency to USD 333.60. Scheduled manual stops are therefore essential for this credit-limited plan. EBS and retained monitoring resources still cost money while the instance is stopped. [2,6–9]

Use only the verified USD 100 credit balance until additional credit is actually granted. The planned gross estimate leaves USD 32.80 headroom before actual pricing differences and tax. Configure a monthly budget alert and review actual gross usage weekly with credits excluded from the view. Budget notifications are not an immediate hard spending cap. [11–12]


### 7 Risk assessment

| Risk | Likelihood and impact | Mitigation and owner |
| --- | --- | --- |
| Credit depletion | Medium / High | Operator stops EC2 and reviews gross costs weekly |
| Instance capacity or plan restriction | Medium / Medium | Operator checks eligible x86_64 type and revises budget |
| Incorrect model or class order | Medium / High | ML developer verifies hashes map and preprocessing |
| CORS or public IP change | High / Medium | Backend operator recreates container with correct origin |
| Access or credential exposure | Medium / High | Operator uses instance role private S3 and restricted SSH |
| Host outage or saturation | Medium / High | Backend operator measures queue load and rehearses recovery |
| Alarm email not delivered | Medium / Medium | Operator confirms SNS and performs threshold test |
| Field data differs from training | High / High | ML developer collects labeled samples and limits claims |

The initial demonstrator accepts single-host downtime and HTTP access restricted to approved client IPs. Public production use requires a separate security and availability review, including HTTPS and authentication. The RAM alarm is intentionally the only operational alarm in this scope and does not cover service availability or ML quality.


### 8 Expected outcomes

| Outcome | Acceptance evidence |
| --- | --- |
| Reproducible release | Two image digests plus checkpoint map hashes and config |
| Working AWS application | Health and prediction succeed; browser prediction and explanation verified |
| Secure artifact retrieval | Private S3 GetObject by instance role with matching hashes |
| Operational visibility | Logs host dashboard one RAM alarm and a received SNS email |
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

[5] CloudWatch alarm actions

[6] EC2 On Demand pricing

[7] EBS pricing

[8] Public IPv4 pricing

[9] CloudWatch pricing

[10] S3 pricing

[11] AWS credits

[12] AWS Budgets
