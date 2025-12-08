---
title: "Week 13 Worklog"
date: "2025-12-01"
weight: 13
chapter: false
pre: " <b> 1.13. </b> "
---
### Week 13 Objectives:
Complete the project and submit
### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Removed Map State in Steps Function <br> - Created Lambdas to add policies to EC2 Instance for SSM automation in IR Step Functions <br> - Reconfigured Quarantine SG: Added Outbound rule for HTTPS for SSM connection <br> - Replaced Lambdas with Step Functions provided States: Used DescribeIamInstanceProfileAssociation, AttachRolePolicy, DetachRolePolicy and StartAutomationExecution <br> - CDK: Created EventBridge and Topics with subscription emails stored in cdk-context <br> - Team meetings: Planned and reassigned task to meet the new deadline | 01/12/2025 | 01/12/2025      |[CDK Tutorial](https://docs.aws.amazon.com/cdk/v2/guide/hello-world.html)|
| 3   | - CDK: Added SES alert for GuardDuty findings <br> - CDK: Added ENI ETL into ETL Pipeline <br> - Assisted in upgrading dashboard <br> Updated Event Participated and overall Worklog update fix and improvement <br> - Researched more on how to optimize pipeline, currently the S3 Get Request is higher than expected do to Athena query low size but many objects | 02/12/2025 | 02/12/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | -CDK: Upgraded Alert with Slack <br> - Architecture: br> &emsp; + Researched and failed in using SQS to pool logs before sending to Lambda: Lambda is still event based and still process log individually instead of pooling <br> &emsp; + Researched and added Data Firehose to consolidate logs before writing to the processed S3 => Reduced the amount of objects written to S3 <br> - IR Step Function revised: Removed SSM actions due to it requiring outbound connections after isolating EC2 => Replaced it with tagging, removing it from ASG and taking a EBS Snapshot to for analyzing and preserving data <br> - Team member updated CloudWatch ETL with Data Firehose succesfully <br> - CDK: Updated all of ETL Pipeline with Kinesis Firehose + Overhauled CloudTrailELT | 03/12/2025 | 03/12/2025  |  |
| 5   | - Partly finished writing Workshop on creating ETL Pipeline <br> CDK: Created and updated Step Functions <br> - Updated Worklog: Events  | 04/12/2025 | 04/12/2025      |  |
| 6   | - Joined the BUILDING AGENTIC AI - Context Optimization with Amazon Bedrock Workshop: Won a prize from CloudThinker for winning in the Workshop <br> - Updated GuardDuty ETL and table for querying optimzation <br> - Redrew and updated Architecture Diagram | 05/12/2025 | 07/12/2025      | [Event Summary and Experience](../../4-EventParticipated/4.7-Event7)|


### Week 13 Achievements:
