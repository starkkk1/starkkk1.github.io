---
title: "Blogs Posted"
date: 2026-10-21
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

# Technical Blogs Shared

During the First Cloud AI Journey internship, I read, analyzed, and shared technical summaries of three AWS architecture and cloud-AI articles with the [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) community. Each post breaks down the underlying AWS architecture, cost-optimization trade-offs, and practical caveats from the original article.

### [Blog 1 — Automating Medical Coding with Step Functions and Amazon Bedrock AgentCore](3.1-Blog1/)
An architectural breakdown of combining deterministic **AWS Step Functions** orchestration with a bounded **Amazon Bedrock AgentCore** agent (backed by MCP tools and **Aurora PostgreSQL + pgvector**) to automate medical coding while maintaining auditability and confidence-based human-in-the-loop routing.

---

### [Blog 2 — Upgrading AI-Powered Game Backends with New Amazon DynamoDB Features](3.2-Blog2/)
A review of four **Amazon DynamoDB** capabilities for stateful AI game backends—**Multi-Region Strong Consistency (MRSC)**, **Zero-ETL to Amazon SageMaker Lakehouse**, **LangGraph DynamoDBSaver**, and **On-Demand / Global Tables pricing reductions**—along with practical constraints and cost-calculation checks.

---

### [Blog 3 — Resolving Security Signals Faster with Arctic Wolf’s Aurora Agentic SOC](3.3-Blog3/)
An analysis of Arctic Wolf's **Aurora Agentic SOC** integration on AWS Marketplace, tracing its event-driven ingestion pipeline (**Security Hub CSPM &rarr; EventBridge &rarr; Data Firehose &rarr; S3 &rarr; Lambda**) and how AI agents handle high-volume triage before escalating complex decisions to human analysts.
