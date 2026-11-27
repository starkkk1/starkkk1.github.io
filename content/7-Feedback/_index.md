---
title: "Sharing and Feedback"
date: 2026-11-26
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

# Sharing and Feedback

## Overall Evaluation

### 1. Learning experience
The most valuable part of this internship has been working through a project that connects machine learning with cloud deployment. I began with tomato leaf images and a classification model, then worked on packaging the application and operating it on AWS. This helped me understand that a useful demonstration depends on more than model output: model files, class mapping, permissions, network access, logs, monitoring, cost and recovery all need attention.

### 2. Relevance to my major
The project was relevant to my Information Technology studies and gave me a chance to apply programming and machine learning in a practical setting. It also introduced me to cloud operations that I had less experience with, including IAM roles, private S3 artifacts, Docker containers on EC2, CloudWatch logs and metrics, and secure web access.

### 3. Skills and professional growth
Troubleshooting access, configuration and deployment issues taught me to verify each layer instead of assuming that a running page means the whole application works. Preparing worklogs, a workshop guide and technical blog posts also helped me explain technical decisions and limitations more clearly. Participating in cloud and AI community events gave me additional perspectives on system resilience, security, cost awareness and responsible use of AI.

### 4. Project challenges and lessons
The project made the limits of a small demonstration clear. A single EC2 instance can become a point of failure, and host monitoring metrics do not prove that predictions are available or accurate. I also learned to distinguish a sample inference time from a proper performance measurement, and to avoid making claims about field accuracy without independent labeled data. In future work, I would plan acceptance checks and operational evidence alongside implementation from the start.

## Additional Questions

**What did I find most satisfying?**  
Connecting the model and web application to AWS, then documenting the configuration and operational lessons, was the most satisfying part. It gave me a more complete view of how an AI feature moves from an experiment toward a usable service.

**What could be improved for future interns?**  
A short milestone checklist with expected deliverables and acceptance criteria would help interns understand what evidence to collect at each stage. Regular review points for architecture, security and deployment would also help surface issues earlier while leaving room for interns to investigate and solve them.

**Would I recommend the program to a friend?**  
Yes, especially to someone interested in cloud and AI who is ready to learn by building and documenting a practical project. A clear project scope and regular feedback would make that experience even more effective.

## Suggestions and Expectations

- Provide a concise project-start guide covering scope, milestones, expected evidence and report structure.
- Include periodic mentor reviews at key stages, such as model integration, AWS permissions, public access and final acceptance.
- Share a cloud cost and cleanup checklist so interns can review running resources, budgets and end-of-project cleanup with confidence.
- Keep opportunities to present project results and exchange feedback with other interns or the AWS Study Group community.

I hope to continue learning through hands-on cloud and AI projects, community knowledge sharing and feedback from experienced practitioners. I am grateful for the opportunity to build, troubleshoot and reflect on this project during the First Cloud AI Journey.
