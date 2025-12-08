---
title: "Week 9 Worklog"
date: "2025-09-09"
weight: 09
chapter: false
pre: " <b> 1.9. </b> "
---
### Week 9 Objectives:

* Keep working on the workshop

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - AWS Architecture revised: <br>&emsp; + Removed AWS Detective <br>&emsp; + Updated with Step Function Workflow instead of a singular AWS Lambda Function <br>&emsp; + Added Custom Dashboard: A static custom dashboard website hosted with S3 and use Athena to query from data lake <br> - School subject: <br> &emsp; + KS57: Completed Quản trị dự án và duy trì đổi mới trong chuyển đổi số  | 03/11/2025 | 03/11/2025 | [Quản trị dự án và duy trì đổi mới trong chuyển đổi số](https://www.coursera.org/account/accomplishments/verify/IC06JCSZ7AVG) |
| 3   | - Successfully exported Guard Duty Findings to S3 Bucket <br> - Experimented with AWS Glue Crawler: <br>&emsp; + Failed on Cloudwatch and CloudTrail logs, schema too complicated for Crawler (Have to research on an alternative way) <br>&emsp; + Ran succesfully on exported Guard Duty Findings: Had to update KMS Policy to allow Crawler to decrypt data <br> - Researching ETL Pipeline for logs| 04/11/2025 | 04/11/2025      | |
| 4   | - Team meetings: Progress report: <br>&emsp; + IR Workflow: Halfway done, EC2 quarantine function is finished, not tested with findings yet <br>&emsp; + Assigned task for dashboard frontend design <br>&emsp; + Assigned task for Glue ETL Pipeline research <br>&emsp; + Signed up for VPBank Cloud Day 2025 with team members | 05/11/2025 | 05/11/2025 || 
| 5   | - Team meetings <br> - Researched on ETL Pipeline approach: <br>&emsp; + Instead of using Glue ELT Jobs, we use custom Lambda ELT pipeline for CloudTrail and CloudWatch  logs <br>&emsp; + Store raw logs into a Raw Log S3 Bucket then use ETL Lambda to process the data and write it to a Proccesed Data S3 to then be Crawled <br> - AWS Architecture revised: Added a new group: DATA PREP group which contain the Raw Log S3 Bucket and the ETL Lambda <br>- School subject: <br> &emsp; + ENW493c: Completed Advanced Writing | 06/11/2025 | 06/11/2025      | [Advanced Writing](https://www.coursera.org/account/accomplishments/verify/EDQ1NY2UG063) |
| 6   | - Researched on Kinesis Data Firehose to collect log: Good for future usage, not suitable for current project because real time streaming data was not necessary, using batch processing is better <br> - Succesfully build an ETL Pipeline for CloudTrail logs: Triggered by object creation in CloudTrail Raw Log Bucket and reformatted the raw logs into JSONL and save it into Proccessed S3 <br> - Succesfully crawled and queried the processed log to show CloudTrail Events | 07/11/2025 | 07/11/2025      | |


### Week 9 Achievements:

* Architecture Refinement:

  * Updated the Incident Response (IR) mechanism to use a Step Functions Workflow instead of a single Lambda function.

  * Introduced a Custom Dashboard (static website hosted on S3) that uses Athena to query the data lake.
  
  * Created a new DATA PREP group in the architecture, including a Raw Log S3 Bucket and an ETL Lambda to manage log transformation.

* Successfully configured the pipeline to export GuardDuty Findings to an S3 Bucket.

* Built and deployed a custom ETL Lambda pipeline** to process CloudTrail logs, triggered by new objects in the Raw Log S3 Bucket.

* Successfully crawled and queried the processed CloudTrail logs using Glue/Athena to demonstrate CloudTrail Events.

* Team members completed half of the IR Step Functions Workflow, with the EC2 quarantine function being finished.

* Assigned tasks for dashboard frontend design and Glue ETL Pipeline research.

* Signed up with team members for the VPBank Cloud Day 2025 event.