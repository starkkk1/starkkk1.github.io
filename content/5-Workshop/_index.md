---
title: "Workshop: Plant Disease Classification on AWS"
date: 2026-10-28T10:00:00+07:00
weight: 5
chapter: false
pre: "<b>5. </b>"
---

# AWS Deployment Workshop Report
## Tomato Leaf Disease Classification with EC2, S3, and CloudWatch

> **Region:** Asia Pacific (Sydney) `ap-southeast-2`  
> **Source Repository:** [starkkk1/plant-disease-detection](https://github.com/starkkk1/plant-disease-detection)  
> **Deployment Status:** Containerized Production Demonstration on AWS

---

### Executive Summary

The **Plant Disease Detection** project has successfully progressed from local PyTorch research to a containerized, production-grade cloud deployment on Amazon Web Services (AWS). 

A single **Amazon EC2** instance hosts a responsive **Next.js** web frontend and a high-performance **FastAPI CPU inference service**. **Amazon S3** provides secure, decoupled storage for versioned deep learning model weights, while **Amazon CloudWatch** and **Amazon SNS** collect real-time operational telemetry, host metrics, and dispatch proactive memory saturation alarms.

![Plant Disease Detection UI Preview](/images/5-Workshop/plant-disease/frontend-mobile.png?width=750px)

---

### Recorded Configuration

| Configuration Item | Recorded Specification |
|---|---|
| **AWS Region** | Asia Pacific Sydney / `ap-southeast-2` |
| **Primary AWS Services** | Amazon EC2, Amazon S3, Amazon CloudWatch |
| **Supporting Services** | AWS IAM, Amazon VPC, Security Groups, Amazon SNS |
| **Application Stack** | Next.js Standalone (Node.js 20), FastAPI, PyTorch (CPU), Docker, GHCR |
| **Deployment Model** | Single host with dual isolated containers; manual release promotion |
| **Machine Learning Architecture** | Knowledge-distilled MobileNetV3-Small (Student) from ConvNeXt-Tiny (Teacher) |
| **Monitoring Scope** | Custom CloudWatch Agent (CPU, RAM, Disk metrics) + Automated RAM Alarm (>85%) |

---

### End-to-End Operational Pipeline

```mermaid
flowchart LR
    Dev[Developer / Git Push] --> GHA[GitHub Actions CI/CD]
    GHA -->|Build & Test| GHCR[GitHub Packages / GHCR]
    
    subgraph AWS_Cloud ["AWS Cloud (ap-southeast-2)"]
        subgraph EC2_Host ["Amazon EC2 Instance"]
            FE["Next.js Frontend Container (:3000)"]
            BE["FastAPI Backend Container (:8000)"]
            CW_Agent["CloudWatch Agent (Host Metrics)"]
            FE -->|Inference Call| BE
        end
        
        S3["Amazon S3 Bucket (Private Checkpoints)"] -->|Mount Read-Only| BE
        BE -->|stdout / stderr| CW_Logs["CloudWatch Logs (/plant-disease)"]
        CW_Agent -->|mem_used_percent| CW_Alarm["CloudWatch RAM Alarm (>85%)"]
        CW_Alarm -->|Trigger| SNS["Amazon SNS Topic (Email Alert)"]
    end
    
    GHCR -->|Deploy Images| EC2_Host
    Client["User Mobile / Desktop Browser"] -->|HTTP :3000| FE
```

---

### Workshop Sections

1. **[5.1 Workshop Overview & Deep Learning System](5.1-workshop-overview/)**  
   Introduction to the 11-class tomato leaf classification problem, knowledge distillation benchmarks, and Grad-CAM++ explainability.
2. **[5.2 Prerequisites & Environment Preparation](5.2-prerequiste/)**  
   AWS account configuration, VPC subnets, client-restricted Security Groups, IAM Instance Profiles, and Docker engine setup.
3. **[5.3 Architecture Design & S3 Model Storage](5.3-architecture-design/)**  
   Decoupled model artifact architecture, S3 bucket security, IAM assumed-role validation, and SHA256 integrity verification.
4. **[5.4 Containerized Application Deployment](5.4-deployment-strategy/)**  
   Multi-stage Docker builds, runtime environment configuration, Next.js standalone serving, and live inference validation with real leaf fixtures.
5. **[5.5 Monitoring and Access Control](5.5-quality-assurance/)**  
   Structured JSON container logging with AWSlogs driver, CloudWatch Agent memory telemetry, automated RAM breach alarms, and least-privilege security.
6. **[5.6 Clean Up and Daily Operations](5.6-operations-cleanup/)**  
   Daily container start/stop procedures, dynamic public IP management, and complete cloud resource teardown checklist.
