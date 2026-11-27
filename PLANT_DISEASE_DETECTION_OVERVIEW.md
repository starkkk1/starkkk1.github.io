# Plant Disease Detection — Project Overview

## Problem Statement

Manually identifying tomato leaf diseases takes time and depends on experience. This project explores image classification as a way to screen tomato leaves into 11 labels: nine disease categories, healthy, and `unknown`. The `unknown` label is a trained class, not a general detector for unfamiliar images.

The main challenge is balancing useful accuracy with low inference cost and the ability to run on modest hardware. Generalization is a concern: the recorded MobileNetV3-Small baseline achieved 98.65% accuracy on 3,195 test images, but 55.22% on a 67-image external set. Macro-F1 was 0.9846 and 0.3952, respectively. The external set is small and imbalanced, so these figures do not establish field reliability. Softmax confidence is not calibrated as the probability of a correct diagnosis.

## Proposed Solution

Build a responsive web application that accepts a JPEG or PNG image from a phone or computer, returns ranked disease predictions, and offers an optional visual explanation.

- **Frontend:** Next.js interface with image preview, progress, cancellation, retry, and model-version details.
- **API:** FastAPI endpoints `GET /health`, `POST /predict`, and `POST /explain`. Uploads are limited to 5 MiB and decoded images to 20 megapixels.
- **Inference model:** MobileNetV3-Small running on CPU with a verified checkpoint and class map. The recorded development-machine benchmark measured 1.529 million parameters, a 5.964 MiB checkpoint, and 8.82 ms p50 / 11.11 ms p95 CPU latency. These timings exclude startup, Grad-CAM++, networking, and cold starts; they are not a production SLA.
- **Explainability:** Grad-CAM++ highlights image regions that contributed to a selected class. It is an attribution aid, not lesion segmentation or a confirmed diagnosis.
- **Deployment:** The current run guide describes frontend and backend containers on one EC2 instance in Sydney (`ap-southeast-2`). Model artifacts are stored in private S3; CloudWatch collects logs and host metrics; SNS sends RAM alerts; GHCR stores container images. EC2 release and rollback steps are manual.

## End-to-End Workflow

1. **Prepare data:** Keep training, test, and external evaluation splits separate. Align labels with the verified `class_to_idx.json`. Dataset and checkpoint files remain outside Git.
2. **Train candidate models:** The research pipeline includes ConvNeXt-Tiny as a teacher and EfficientNet-B0 and MobileNetV3-Small students. Students can be trained independently or with Knowledge Distillation; some runs also use MixUp and CutMix.
3. **Evaluate:** Compare accuracy, macro-F1, precision, recall, confusion matrices, per-class errors, checkpoint size, parameter count, and CPU p50/p95 latency. Broader external data is needed before making field-performance claims.
4. **Build and check:** GitHub Actions runs Python checks, builds the Next.js app and containers, and smoke-tests the frontend. Successful runs on `master` publish images to GHCR with commit-specific tags. CI does not deploy to EC2.
5. **Deploy manually:** Select image digests, download the model and class map from private S3, verify SHA-256 hashes, and mount the artifacts read-only. Confirm the backend health check reports a real model before starting the frontend.
6. **Predict:** The browser validates the file and sends a multipart request. The backend checks MIME type, byte size, and pixel count, then converts the image to RGB at 224 × 224 and applies training normalization. The predictor returns ranked labels, softmax scores, processing time, request ID, and model version. Errors do not trigger an automatic mock result.
7. **Explain and monitor:** On request, `/explain` runs Grad-CAM++ and returns a heatmap and overlay. Operators review health, CloudWatch logs, metrics, alerts, and prediction errors. Model artifacts, class map, configuration, and hashes should travel together; deployment and rollback remain manual.

## Additional (Optional)

- **Research image search:** A separate Qdrant service and `/research` page support optional similar-image search.
- **Distillation experiments:** In the recorded baseline, both distilled students scored below their independent counterparts on the test set. Distillation needs measurement; it does not guarantee improvement.
- **Mobile delivery:** The current product is a responsive website. PWA installation, offline inference, and a native Flutter app are future options, not implemented features.
- **Public access:** The documented demo uses HTTP and has no application authentication or high availability. HTTPS, access controls, abuse protection, dependency review, and physical-device checks are needed before public use.

## Cost

The project's monthly AWS operating budget is **USD 50 before tax** for the low-traffic demo setup below.

| Service / usage | Monthly cost |
|---|---:|
| EC2 Linux `t3.medium` in Sydney (`ap-southeast-2`), running 730 hours | $38.54 |
| EBS gp3, 20 GiB | $1.60 |
| Public IPv4, 730 hours | $3.65 |
| S3 model checkpoint and class map | $0.01 |
| CloudWatch logs, host metrics, dashboard, and one RAM alarm | $0.00 |
| SNS email alerts (up to 1,000 email notifications) | $0.00 |
| Internet data transfer (within 100 GB/month shared allowance) | $0.00 |
| Operating headroom | $6.20 |
| **Monthly total** | **$50.00** |

This budget covers one EC2 host running both web and API containers, a 20 GiB disk, one public IPv4 address, small model artifacts, seven-day log retention, and low demo traffic. It excludes tax, GPU training, a load balancer, NAT Gateway, database, high availability, traffic above the stated allowance, excess log/metric usage, and T3 CPU-credit surcharges. Model training and research runs use a local machine; GHCR and GitHub Actions remain within included account usage.

Stopping EC2 when the demo is offline reduces compute charges, but EBS storage remains billable. Use the [AWS Pricing Calculator](https://calculator.aws/) if the region or configuration changes. AWS lists a $0.005 hourly charge per public IPv4 address; low-volume CloudWatch usage and up to 1,000 SNS email notifications per month fall within published free allowances ([IPv4 pricing](https://aws.amazon.com/vpc/pricing/), [CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/), [SNS pricing](https://aws.amazon.com/sns/faqs/)).

With **USD 200 in eligible AWS credits**, this budget covers **four months** of continuous operation. AWS advertises $100 at sign-up and up to another $100 for qualifying activities for eligible new customers. Credit eligibility, balance, and expiration depend on the account; check the AWS Billing Console before using credits in a project budget ([AWS Free Tier](https://aws.amazon.com/free/), [Free Tier FAQ](https://aws.amazon.com/free/free-tier-faqs/)).

