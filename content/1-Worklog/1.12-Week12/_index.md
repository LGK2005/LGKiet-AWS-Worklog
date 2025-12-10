---
title: "Week 12 Worklog"
date: "2025-11-24"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Week 12 Objectives:

* Optimize Athena queries and dashboard performance.
* Improve dashboard UI/UX and deployment pipeline.
* Begin defining the dashboard infrastructure with AWS CDK.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | Joined the team GitHub organization instead of creating a new one, and organized repositories for the dashboard and backend services. <br> Cleaned up repo structure and permissions to prepare for future collaboration. | 24/11/2025 | 24/11/2025 | - |
| Tue | Applied partition projection to Athena tables to optimize queries: <br> &emsp; + Reviewed existing partitions and table definitions for cost/performance data. <br> &emsp; + Configured partition projection properties on the Athena table to reduce metadata lookups. <br> &emsp; + Tested queries before and after partition projection to confirm faster planning and lower scanned data. | 25/11/2025 | 25/11/2025 | [Athena partition projection](https://docs.aws.amazon.com/athena/latest/ug/partition-projection.html) |
| Wed | Improved the dashboard UI: <br> &emsp; + Refined layout for key panels (threats, IR workflow status, cost metrics). <br> &emsp; + Adjusted color palette, typography, and spacing for better readability. <br> &emsp; + Polished charts/cards to make the data easier to scan during demos. | 26/11/2025 | 26/11/2025 | - |
| Thu | Migrated dashboard repository from GitHub to GitLab and set up GitLab CI: <br> &emsp; + Imported the existing React dashboard project into GitLab. <br> &emsp; + Created a `.gitlab-ci.yml` pipeline to build the project and deploy static files to S3. <br> &emsp; + Added steps to invalidate the CloudFront distribution after each successful deployment. | 27/11/2025 | 27/11/2025 | [GitLab CI/CD to S3 & CloudFront](https://blog.logicwind.com/auto-deploy-spa-with-aws-s3-and-cloudfront-using-gitlab-ci-cd/) |
| Fri | Started writing AWS CDK code for the dashboard stack: <br> &emsp; + Created a new CDK app and stack dedicated to the dashboard. <br> &emsp; + Defined S3 bucket resources for static hosting of the dashboard. <br> &emsp; + Added initial IAM policies required for deployment and access to the bucket (to be expanded later). | 28/11/2025 | 30/11/2025 | [AWS CDK Developer Guide](https://docs.aws.amazon.com/cdk/v2/guide/home.html) |

### Week 12 Achievements:

* Organized code collaboration by joining the existing GitHub organization and structuring repos for the project.
* Improved Athena performance by enabling partition projection on key tables, reducing query planning overhead and scanned data.
* Enhanced the dashboard’s UI for readability and demo readiness, and migrated its deployment pipeline from GitHub to GitLab CI with S3 + CloudFront.
* Bootstrapped an AWS CDK stack for the dashboard, starting with S3 and IAM definitions to move towards full infrastructure-as-code.
