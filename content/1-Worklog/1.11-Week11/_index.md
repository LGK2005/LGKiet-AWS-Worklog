---
title: "Week 11 Worklog"
date: "2025-09-09"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Refine and connect project components (dashboard, API, data layer).
* Prepare for infrastructure-as-code using AWS CDK.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | Joined the AWS Cloud Mastery Series #2 – DevOps on AWS. <br> Gained valuable insights into using CloudFormation and CDK for managing infrastructure. <br> Received mentor recommendations about demo strategy for the project and wrote up event experience notes. | 17/11/2025 | 17/11/2025 | [Event Summary and Experience](../../4-EventParticipated/4.3-Event3) |
| Tue | Set up API Gateway and a simple Lambda function to test calling APIs from the dashboard: <br> &emsp; + Created a REST API in API Gateway with a Lambda proxy integration. <br> &emsp; + Implemented a basic Lambda handler returning test JSON data. <br> &emsp; + Wired the React dashboard to call the API endpoint and verified end-to-end connectivity. | 18/11/2025 | 18/11/2025 | [Lambda proxy integration in API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-lambda-proxy-integrations.html) |
| Wed | Updated the Lambda function to query data using Athena: <br> &emsp; + Implemented a Lambda that submits Athena queries and polls for completion. <br> &emsp; + Configured output location for Athena query results in S3. <br> &emsp; + Verified that Lambda can return query results to the dashboard via API Gateway. | 19/11/2025 | 19/11/2025 | [How to query Athena from Lambda](https://www.youtube.com/watch?v=a_Og1t3ULOI) |
| Thu | Refined the external architecture around the dashboard: <br> &emsp; + Removed Route 53 from the architecture for this phase and focused on CloudFront as the main entry point. <br> &emsp; + Tested connectivity and flow between CloudFront, API Gateway, and Lambda. <br> &emsp; + Updated diagrams and notes to reflect the simplified routing path. | 20/11/2025 | 20/11/2025 | - |
| Fri | Researched AWS CDK in preparation for codifying the architecture: <br> &emsp; + Reviewed how to install and bootstrap AWS CDK. <br> &emsp; + Studied basic concepts: App, Stack, Constructs, and how to define infrastructure in code. <br> &emsp; + Looked at examples of defining API Gateway, Lambda, and S3/CloudFront resources using CDK. | 21/11/2025 | 23/11/2025 | [AWS CDK GitHub](https://github.com/aws/aws-cdk) <br><br> [AWS CDK Developer Guide](https://docs.aws.amazon.com/cdk/v2/guide/home.html) |

### Week 11 Achievements:

* Learned CDK and CloudFormation concepts from AWS Cloud Mastery Series #2 and gathered demo strategy feedback from mentors. [web:14]
* Successfully connected the React dashboard to a REST API built with API Gateway and Lambda, verifying proxy integration and basic JSON responses. [web:14]
* Implemented a Lambda function capable of running Athena queries and returning results, enabling data-driven views in the dashboard. [web:23]
* Simplified and validated the external architecture by testing the flow CloudFront → API Gateway → Lambda without Route 53, and prepared to codify this in AWS CDK. [web:14][web:23]
