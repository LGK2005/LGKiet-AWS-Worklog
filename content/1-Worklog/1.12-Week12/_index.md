---
title: "Week 12 Worklog"
date: "2025-11-24"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---
### Week 12 Objectives:

* Connect and get acquainted with members of First Cloud Journey.
* Understand basic AWS services, how to use the console & CLI.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Successfully installed AWS CDK with AWS CLI <br> Completed the tutorial for creating a first application with CDK: <br>&emsp; + Deployed stacks on AWS Accounts <br>&emsp; + Used diff to compare changes <br>&emsp; + Destroyed stack after finishing <br> - Created a Github Organization for the team | 24/11/2025 | 24/11/2025      |[CDK Tutorial](https://docs.aws.amazon.com/cdk/v2/guide/hello-world.html)|
| 3   | - Added to IR Step Functions: Added a Map State to illiterate isolated Instances and trigger SSM Lambda for those Instances to collect logs from them for forensics <br> - Succesfully helped with creating auto export CloudWatch logs: Used Lambda to parse subcription filter form log stream to Raw Log S3 bucket, will have to modify CloudWatch ETL Lambda to work with the new auto export rather than the batch export job <br> - Update CloudTrail ETL Lambda: Noticed the usually high storage cost in the Processed CloudTrail Log bucket => The current Lambda Function save files as unzipped .jsonl => Updated the function so that the files is compressed by gzip before saving <br> - CloudTrail ETL Lambda have some spiking and error invocations when multiple people is interacting with the account => Raised timeout limit <br> CDK: Moved the CDK testing environment to a new account <br> - CDK: Created a Stack that enables GuardDuty and CloudTrail and the Raw Log S3 Bucket   | 25/11/2025 | 25/11/2025      ||
| 4   | - CDK: Updated Bucket and CloudTrail Policy to replicate the current infastructure: got into circular dependencies but is resolved <br> CDK: Successfully recreated CloudTrailETL pipeline with Raw and Processed log buckets, ETL Lambda and Glue table to be queried with Athena and set the related polices | 26/11/2025 | 26/11/2025      | |
| 5   | - CDK: Configured CloudWatch, Log Group, Dns Query logging, added cdk-context for user to enter VPC ids to add logging for analysis <br> - Optimization: CloudTrail logs have gotten too much, quick check reveals that it also logs S3 Put events from the ETL Lambdas, causing a loop => Created custom event exclusion in CloudTrail Events tab to exclude APIs called by the ETL Lambdas <br> - Exclude event by Lambdas ARN wasn't reliable => Exclude API from log buckets <br> - CDK: Its impossible to configure advanced event selectors in CDK so that will have to be removed <br>- CDK: Succesfully configured the CloudWatch Auto Export Lambda and Subscription Filter: Got a lot of permission error from Subscription Filter permission to invoke Lambda => Used L2 construct and explicit dependency for _create_subscription_filter  | 27/11/2025 | 27/11/2025 | |
| 6   | - CDK: Added ClouWatch ETL and the related Glue Table and Processed Bucket  <br> -CDK: Added KMS Key to allow GuardDuty to export findings to S3 BUcket and added the GuardDutyETL to process the findings for querying => Fully completed the ETL Pipeline and Data Forensics <br> - Team meetings: <br>&emsp; + Assigned CDK tasks for members <br>&emsp; + Got started on updating proposal and architecture diagram <br> - Fixed and improved IR Steps Function: <br>&emsp; + Fixed EC2Isolate Lambda: Wrong parsing method <br>&emsp; + Improved state: Added Parsing Lambda and reordered functions <br>&emsp; + SSM Failed due to missing IAM: Add role will be added into the SSM Forensics Function <br> - Joined the AWS Cloud Mastery Series #3: AWS Well-Architected – Security Pillar Workshop   | 28/11/2025 | 30/11/2025  |[Event Summary and Experience](../../4-EventParticipated/4.6-Event6)|


### Week 12 Achievements:
