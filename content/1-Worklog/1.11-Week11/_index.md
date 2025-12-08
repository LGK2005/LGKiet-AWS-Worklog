---
title: "Week 11 Worklog"
date: "2025-09-09"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Refine project.


### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Joined the AWS Cloud Mastery Series #2 - DevOps on AWS: Got really good values in CDK and CloudFormation for the project, got some recommendation from mentors about demo strategy, wrote about event experience | 17/11/2025 | 17/11/2025      |[Event Summary and Experience](../../4-EventParticipated/4.4-Event4)|
| 3   | - Dashboard overview: Updated for more upfront fields <br> - Architecture revised: <br>&emsp; + Removed Crawler from the architecture <br>&emsp; + Added SQS between EventBridge and StepFunctions  <br> - Took note of the cost of S3 API calls, especially the S3 Bucket for CloudTrail logs, the Lambda is configured to process every CloudTrail logs, resulting in large number of S3 GET calls, have to update the pricing accordingly <br> - Team member succesfully Isolated EC2 in testing environment <br> - Succesfully upgraded GuardDuty ETL Lambda to create table without Crawler and return more detailed fields <br> - Team member succesfully upgraded CloudWatch ELT Lambda to create table without Crawler, I helped in creating a trigger and update Lambda code to process new exported file <br> Backed up Lambda codes| 18/11/2025 |18/11/2025 ||
| 4   | - Joined the Secure Your Applications: AWS Perimeter Protection Workshop: Learnt more about CloudFront and WAF and got introduced to the brand new CloudFront pricing tier, did two workshops on CloudFront and WAF <br> - Learnt how to set up API Gateway RestAPIs to prepare to integrate with dashboard | 19/11/2025 | 19/11/2025  |[Event Summary and Experience](../../4-EventParticipated/4.5-Event5)|
| 5   | - Invited back to school for Teacher's Day <br> - Family matters | 20/11/2025 | 20/11/2025      ||
| 6   | - Invited to FPT's Convocation Day by graduating bachelors <br> - Researched on CDK: How to install, how to use, how to configure stacks to prepare for next week's plan| 21/11/2025 | 23/11/2025 | [AWS CDK Github](https://github.com/aws/aws-cdk) <br><br> [AWS CDK Document](https://docs.aws.amazon.com/cdk/v2/guide/home.html) |


### Week 11 Achievements:
