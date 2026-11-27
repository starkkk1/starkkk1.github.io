---
title: "Workshop"
date: 2026-10-28T10:00:00+07:00
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# AWS Deployment Workshop Report
## Tomato Leaf Disease Classification with EC2, S3, and CloudWatch

> **Region:** Asia Pacific (Sydney) `ap-southeast-2`  
> **Source Repository:** [starkkk1/plant-disease-detection](https://github.com/starkkk1/plant-disease-detection)  
> **Deployment Status:** Single-instance AWS demonstration deployment
> **Target Workshop Completion:** November 27, 2026 (before the November 30 deadline)

---

### Executive Summary

The **Plant Disease Detection** project has progressed from local PyTorch experiments to a working containerized demonstration on Amazon Web Services (AWS). The deployment shows how the model can be served through a web application and monitored on a small cloud setup; it is not a production service or a validated agricultural diagnostic product.

A single **Amazon EC2** instance runs a responsive **Next.js** frontend and a **FastAPI CPU inference service** in separate containers. **Amazon S3** stores the model artifacts, while **Amazon CloudWatch** and **Amazon SNS** provide logs, host metrics, and a RAM threshold email alert. Deployment and rollback are manual. The setup has one host, limited access controls, and no high availability; model results have not been validated for field use.

![Plant Disease Detection UI Preview](/images/5-Workshop/plant-disease/frontend-mobile.png?width=750px)

---

### Recorded Configuration

| Configuration Item | Recorded Specification |
|---|---|
| **AWS Region** | Asia Pacific Sydney / `ap-southeast-2` |
| **Primary AWS Services** | Amazon EC2, Amazon S3, Amazon CloudWatch |
| **Supporting Services** | AWS IAM, Amazon VPC, Security Groups, Amazon SNS |
| **Application Stack** | Next.js Standalone (Node.js 22), FastAPI, PyTorch (CPU), Docker, GHCR |
| **Deployment Model** | One EC2 host with two containers; manual deployment and rollback |
| **Machine Learning Architecture** | Knowledge-distilled MobileNetV3-Small (Student) from ConvNeXt-Tiny (Teacher) |
| **Monitoring Scope** | Custom CloudWatch Agent (CPU, RAM, Disk metrics) + Automated RAM Alarm (>85%) |

---

### End-to-End Operational Pipeline

```mermaid
graph LR
    Dev["Developer / Git Push"] -->|push master| GHA["GitHub Actions: pytest, build, smoke"]
    GHA -->|publish| GHCR["GHCR: api + frontend"]
    Local["Local Workstation: Training & Audit"] -->|upload| S3

    subgraph AWS Cloud ap-southeast-2
        subgraph EC2 Host Amazon Linux 2023
            FE["frontend: Next.js :3000"]
            BE["backend: FastAPI :8000"]
            CW_Agent["CloudWatch Agent: 60s"]
        end

        S3["Amazon S3: Private Bucket"] -->|aws s3 cp + sha256| BE
        IAM["AWS IAM: Instance Role"] -.->|instance profile| BE
        FE -->|awslogs| CW["Amazon CloudWatch"]
        BE -->|awslogs| CW
        CW_Agent -->|mem + disk| CW
        CW -->|RAM > 85pct, 3x1 min| SNS["Amazon SNS: plant-disease-alerts"]
    end

    GHCR -->|manual docker pull sha256| FE
    GHCR -->|manual docker pull sha256| BE
    Client["Users / Browser"] -->|GET :3000| FE
    Client -->|POST :8000| BE
    SNS -->|email alert| Dev
```

---

### Workshop Sections

1. **[5.1 Introduction](5.1-workshop-overview/)**
   Introduction to the 11-class tomato leaf classification problem, knowledge distillation benchmarks, and Grad-CAM++ explainability.
2. **[5.2 Prerequisites & Setup](5.2-prerequiste/)**
   AWS account configuration, VPC subnets, client-restricted Security Groups, IAM Instance Profiles, and Docker engine setup.
3. **[5.3 Architecture & Design](5.3-architecture-design/)**
   Decoupled model artifact architecture, S3 bucket security, IAM assumed-role validation, and SHA256 integrity verification.
4. **[5.4 Deployment Strategy](5.4-deployment-strategy/)**
   Multi-stage Docker builds, runtime environment configuration, Next.js standalone serving, and live inference validation with real leaf fixtures.
5. **[5.5 Testing & Quality](5.5-quality-assurance/)**
   Structured JSON container logging with AWSlogs driver, CloudWatch Agent memory telemetry, automated RAM breach alarms, and least-privilege security.
6. **[5.6 Operations & Cleanup](5.6-operations-cleanup/)**
   Daily container start/stop procedures, dynamic public IP management, and complete cloud resource teardown checklist.
