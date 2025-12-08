---
title: "Proposal"
date: "2025-10-16"
weight: 02
chapter: false
pre: " <b> 2. </b> "
---
# Proposal: Automated AWS Incident Response and Forensics Workshop

### 1. Executive Summary
The Automated Incident Response and Forensics Workshop is designed to provide security and operations teams with hands-on experience building an AWS-native, cost-optimized security automation pipeline. The revised architecture leverages **Amazon GuardDuty** for detection, instantly orchestrates response workflow that automatically selects and executes the correct containment strategy based on the type of security finding via **AWS Step Functions**, and utilizes a robust, modern data pipeline with **AWS Glue and Athena** for highly efficient forensic analysis and visualization via a **CloudFront/S3 Dashboard**. The system is configured for high automation and minimal operational cost, making it ideal for an enterprise pilot.

### 2. Problem Statement
#### What’s the Problem?
Traditional security operations rely on manual log review and human intervention, leading to high Mean Time To Respond (MTTR) and complex, non-standard containment actions. Manual incident investigation is slow and expensive due to unoptimized querying of massive log files.

#### The Solution
The proposed solution implements a complete Detection-to-Containment-to-Forensics lifecycle using serverless AWS services:
- **Detection**: Managed threat intelligence via GuardDuty eliminates the need for complex custom rule writing.
- **Orchestration**: **AWS Step Functions**, triggered by Amazon EventBridge, provides a state machine for orchestrating complex, branching incident response actions based on the threat type (IAM vs. EC2).
- **Containment**: Lambda and AWS SSM are executed via Step Functions to instantly quarantine compromised EC2 instances and disable suspicious IAM keys.
- **Alerting & Collaboration**: Amazon SNS and the **Notification Lambda (Alert Dispatch)** ensure real-time, immediate alerts are pushed to the **3rd party messaging platform (e.g., Telegram)**, enabling **immediate team communication**.
- **Forensics & Reporting**: **CloudWatch** aggregates logs, an **ETL Lambda** processes raw data, and S3, Glue, and Athena form a highly cost-optimized data lake. Reports are delivered globally via **CloudFront** and an **S3 Static Dashboard**.

#### Benefits and Return on Investment
- **Reduced MTTR**: Complex, multi-step containment is standardized and executed in **seconds** using the Step Functions workflow.
- **Real-time Communication**: **Instant, out-of-band notification** via 3rd party messaging ensures the security team is aware and can **collaborate immediately**, bypassing potentially compromised email or network systems.
- **Cost Efficiency**: AWS Glue optimizes S3 logs into partitioned, columnar data (Parquet), drastically cutting the data scanned and thus minimizing Athena query costs.
- **Global Insight**: **CloudFront** distributes the security dashboard globally for fast, reliable reporting.
- **Monthly Costs**: Estimated low recurring cost of **~$10.12 USD** (revised), demonstrating responsible cloud financial management.

### 3. Solution Architecture
The architecture implements a serverless, event-driven pipeline that covers the entire IR lifecycle.
 ![AWS Incident Response and Forensics Architecture](/images/2-Proposal/AWSWorkshopArchitecture-DataPrep.jpg)
_AWS Incident Response and Forensics Architecture_

#### AWS Services Used
| Service | Purpose in IR Lifecycle |
|---|---|
| **AWS Step Functions** | **Orchestration**: Central workflow engine that coordinates branching response actions based on finding type (e.g., IAM vs. EC2). |
| **AWS GuardDuty** | **Detection**: Analyzes CloudTrail & VPC Flow Logs for threats, generates high-fidelity findings. |
| **Amazon EventBridge** | **Event Routing**: Receives GuardDuty findings and routes them to the Step Functions workflow and the alerting pipeline. |
| **AWS Lambda (SSM Forensic)** | **Containment/Forensics**: Executes immediate containment actions and triggers secure remote execution via SSM to collect evidence. |
| **AWS Lambda (ETL)** | **Data Transformation**: Runs the Extract, Transform, Load process to convert raw logs into an optimized, queryable format. |
| **Amazon CloudWatch** | **Log Aggregation**: Central hub for collecting and monitoring VPC Flow Logs and CloudTrail data before processing. |
| **Amazon S3** | **Storage**: Immutable Data Lake for all historical log data and host for the static dashboard. |
| **AWS Glue (Crawler)** | **Optimization**: Catalogs and partitions processed log data, enabling efficient, low-cost querying. |
| **Athena** | **Analysis**: Serverless SQL engine for querying S3 logs during forensics. |
| **API Gateway** | **Integration**: Provides secure HTTPS endpoints for external systems to query Athena results and populate the dashboard. |
| **CloudFront** | **Delivery**: Distributes the S3 Static Dashboard globally with low latency and high availability. |
| **Route 53** | **DNS**: Provides reliable, managed DNS resolution for the CloudFront distribution and any other infrastructure. |
| **Amazon SNS** | **Alerting**: Publishes GuardDuty findings for multi-channel delivery. |
| **Notification Lambda (Alert Dispatch)** | **Custom Channel**: Translates SNS alerts into messages for a **3rd party messaging platform (e.g., Telegram)** to enable **real-time team communication and ChatOps.** |
| **EC2 Instance** | **Target**: The asset being monitored and quarantined. |

### 4. Technical Implementation
#### Implementation Phases (6 Weeks)
The phases now reflect the development of the Step Functions workflow and the setup of the enhanced data pipeline.
- **Week 1-2 (Foundation & Data Prep)**: Enable core logging (CloudTrail, VPC Flow Logs, EC2 logs to **CloudWatch**). Activate GuardDuty. Set up the log aggregation (CloudWatch to S3), **ETL Lambda**, and initial Glue Crawler configuration.
- **Week 3-4 (Advanced Automation)**: Design and deploy the **Step Functions workflow** for orchestration, including the decision logic for IAM vs. EC2 findings. Finalize the Lambda/SSM components for containment actions. Configure EventBridge to trigger the Step Functions state machine.
- **Week 5-6 (Forensics, Reporting & Launch)**: Configure **API Gateway** to query Athena results. Deploy the **S3 Static Dashboard**, configure **CloudFront** distribution, and set up **Route 53** for domain resolution. Conduct a full simulation of an intrusion and automated response.

#### Technical Requirements
- **Detection/Orchestration**: GuardDuty configuration. 
- **Step Functions** State Language (ASL) and decision/branching logic for IAM vs EC2 threats. Configuration of EventBridge rules for event routing.
- **Response Logic**: Python SDK knowledge within Lambda to call the EC2 and IAM APIs. Finalize AWS SSM runbooks for forensic data collection.
- **Forensics/Reporting**: Proficiency in setting up S3, **CloudWatch**, **ETL Lambda**, and Glue for a data lake (including partitioning). Setting up **API Gateway** and **CloudFront** for secure, global report delivery.

### 5. Timeline & Milestones
| Timeline | Key Milestones and Achievements |
|---|---|
| **Week 1-2: Data Prep & Foundation** | <ul><li>All foundational logging (CloudTrail, VPC Flow Logs) enabled, data flowing through **CloudWatch**.</li><li>Amazon GuardDuty fully enabled and generating sample findings.</li><li>**ETL Lambda** developed and successfully transforming raw logs to Parquet in S3.</li></ul> |
| **Week 3-4: Advanced Automation** | <ul><li>**Step Functions workflow** deployed, including **decision logic** for IAM vs. EC2 threats.</li><li>Containment state machine transitions (SSM Forensic, Disable IAM Key, EC2 Quarantine) validated.</li><li>Notification Lambda deployed and integrated with **3rd party messaging platform**.</li></ul> |
| **Week 5-6: Forensics, Reporting & Launch** | <ul><li>AWS Glue Crawler configured and running weekly to catalog processed logs.</li><li>**API Gateway** configured to provide secure access to Athena query results.</li><li>**S3 Static Dashboard** deployed and distributed via **CloudFront** and **Route 53** DNS.</li><li>Full simulation of intrusion → detection → containment → forensics completed.</li></ul> |

### 6. Budget Estimation

The budget estimate was calculated on [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=6b763a94cdaf502737f0797f62cc374ca3d289a4).

| Infrastructure Costs | Assumption | Cost/Month (USD) |
|---|---|---|
| EC2 Instance (t3.micro) | 168 hours/month (7 days runtime) | $2.88 |
| Amazon GuardDuty | Continuous monitoring, minimal log volume | ~$2.08 |
| AWS Glue (Crawler) | 3 Glue Crawlers: One each for CloudWatch, CloudTrail and GuardDuty log (4 weekly runs @ 10 min each + On Demand runs = 6 runs/months)  | $1.32 |
| Amazon S3 | 5GB free + 3GB extra | $0.21 |
| AWS Lambda (IR Orchestrator + ETL) | 1 million free requests + compute charges  | $0.02 |
| AWS Step Functions | 1,000 state transitions/month | $0.00 |
| Amazon Athena | 50 queries per month and 1gb of data scanned per query | $0.24 |
| API Gateway | 1 million requests/month (free tier) + minimal overage | $0.05 |
| CloudFront | 5GB Data Transfer (Always Free Tier) | $0.00  |
| Amazon CloudWatch | 10 metrics and 5GB log ingestion Free Tier + 1GB Extra | $0.83 |
| Route 53 | 1 hosted zone | $0.50 |
| **TOTAL ESTIMATED MONTHLY COST** | | **~$8.13 USD** |
| **TOTAL ESTIMATED ANNUAL COST** | | **~$97.56 USD** |

### 7. Risk Assessment
| Risk | Impact | Probability | Mitigation Strategies |
|---|---|---|---|
| **GuardDuty Cost Spike** (Due to high event volume) | Medium | Medium | Implement strict budget alarms; utilize the 30-day Free Trial to establish a cost baseline. |
| **Step Functions Logic Failure** (Workflow fails to complete) | High | Low | Robust error handling and catch states within the Step Functions State Machine Definition; Lambda functions log failure to a Dead-Letter Queue (DLQ). Immediate manual intervention. |
| **Unoptimized Athena Query** | Medium | Medium | Strict requirement to use AWS Glue partitioning and Parquet conversion (via ETL Lambda) to minimize data scanned. |

### 8. Expected Outcomes
#### Technical Improvements:
- Achieve an **Automated MTTR** (Mean Time To Respond) for EC2 quarantine and IAM disablement measured in seconds, orchestrated by the **Step Functions workflow**.
- Establish an AWS-native, cost-optimized **Security Data Lake and a global reporting platform** via CloudFront/S3.
- Gain hands-on expertise with key security and analysis tools: GuardDuty, **Step Functions**, Glue, Athena, **API Gateway**, and **CloudFront**.

#### Long-term Value:
- Provides a reusable, security-hardened **reference architecture** for future production cloud environments.
- Generates a foundational dataset of security logs suitable for **AI/ML security research** (anomaly detection model training).