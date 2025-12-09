---
title: "Week 7 Worklog"
date: "2025-10-20"
weight: 07
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Learn how to build a secure serverless static website with SSL on AWS.
* Build a simple front-end that calls API Gateway and Lambda in a serverless architecture.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | Serverless – SSL S3 Static Website: <br> &emsp; + Studied the overall architecture for hosting a static website on S3 with HTTPS using ACM, Route 53, and CloudFront. <br> &emsp; + Reviewed the preparation steps and prerequisites for the workshop. <br> &emsp; + Learned how ACM is used to provision and manage SSL/TLS certificates for CloudFront. <br> &emsp; + Reviewed how Route 53 hosted zones and custom domains are used to route traffic to the CloudFront distribution. | 20/10/2025 | 20/10/2025 | [Serverless – SSL S3 Static Website](https://000082.awsstudygroup.com/vi/) |
| Tue | Serverless – Build Frontend to call API Gateway (Part 1): <br> &emsp; + Reviewed the overall architecture: static front-end in S3/CloudFront calling API Gateway, Lambda, and DynamoDB. <br> &emsp; + Studied the introduction and requirements of the workshop. <br> &emsp; + Deployed the front-end application according to the workshop instructions. <br> &emsp; + Verified that the static front-end is accessible and ready to be wired to the API. | 21/10/2025 | 21/10/2025 | [Serverless – Build Frontend to call API Gateway](https://000079.awsstudygroup.com/vi/1-introduce/) |
| Wed | Serverless – Build Frontend to call API Gateway (Part 2): <br> &emsp; + Reviewed how the Lambda function and DynamoDB table are used behind API Gateway. <br> &emsp; + Studied and configured API Gateway routes and integrations for the workshop. <br> &emsp; + Tested the API with Postman and then from the front-end, confirming end-to-end communication. <br> &emsp; + Reviewed cleanup steps to remove API Gateway, Lambda, and related resources after testing. | 22/10/2025 | 22/10/2025 | [Serverless – Build Frontend to call API Gateway](https://000079.awsstudygroup.com/vi/) |
| Thu | Team meeting: quick AWS services revision and discussion about proposal changes. <br> Updated AWS architecture to add AWS Detective. <br> Revised proposal to include AWS Detective and a post-workshop CDK plan. <br> Configured EventBridge rules to react to GuardDuty findings, sending SNS emails to team members and triggering a simple Lambda function. <br> Drafted an idea for a simple S3-hosted data graphing page using API Gateway, Lambda, and Athena for forensics data. | 23/10/2025 | 23/10/2025 | EventBridge, SNS, Lambda, Detective docs |
| Fri | Tried AWS Card Clash with team members to review AWS services and their use in architectures. <br> Reviewed AWS services knowledge for the mid-term by generating quizzes with an LLM based on given requirements. | 24/10/2025 | 24/10/2025 | [AWS Card Clash](https://aws.amazon.com/training/digital/aws-card-clash/) |

### Week 7 Achievements:

* Learned how to host a secure, SSL-enabled static website on S3 using ACM, Route 53, and CloudFront.
* Built an understanding of serverless web architectures where a static front-end calls API Gateway, Lambda, and DynamoDB.
* Advanced the workshop proposal and architecture with AWS Detective and automated incident handling using EventBridge, SNS, and Lambda.
* Strengthened AWS services knowledge through AWS Card Clash and structured revision for the mid-term.
