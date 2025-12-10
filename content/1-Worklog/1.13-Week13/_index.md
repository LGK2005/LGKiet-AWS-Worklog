---
title: "Week 13 Worklog"
date: "2025-12-01"
weight: 13
chapter: false
pre: " <b> 1.13. </b> "
---

### Week 13 Objectives:
Complete the project and submit.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | Continued writing AWS CDK for the dashboard backend and edge layer: <br> &emsp; + Extended the existing CDK app to define Lambda functions used by the dashboard APIs. <br> &emsp; + Added CloudFront distribution configuration for serving the dashboard and routing API paths. <br> &emsp; + Defined API Gateway resources and integrations with Lambda in CDK. | 01/12/2025 | 01/12/2025 | [CDK Lambda + API tutorial](https://docs.aws.amazon.com/lambda/latest/dg/lambda-cdk-tutorial.html) |
| Tue | Added Amazon Cognito into the architecture to support login for the dashboard: <br> &emsp; + Designed where Cognito user pool fits between the front-end and API Gateway. <br> &emsp; + Reviewed authentication flow and how tokens will be used to protect APIs. <br> &emsp; + Updated the architecture diagram to include Cognito and adjusted notes for future implementation. | 02/12/2025 | 02/12/2025 | [Amazon Cognito user pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools.html) |
| Wed | Tested CDK deployment and debugged issues: <br> &emsp; + Ran `cdk synth` and `cdk deploy` for the dashboard-related stacks. <br> &emsp; + Fixed logical and permission errors (IAM policies, missing environment variables, incorrect references). <br> &emsp; + Re-deployed until the CloudFront, API Gateway, and Lambda resources worked as expected. | 03/12/2025 | 03/12/2025 | - |
| Thu | Merged stacks from team members and tested combined deployment: <br> &emsp; + Pulled CDK stacks from other members in the organization. <br> &emsp; + Integrated their stacks (ETL, IR workflow, security components) with the dashboard stack. <br> &emsp; + Deployed the combined app and validated that all stacks work together without conflicts. | 04/12/2025 | 04/12/2025 | - |
| Fri | Family matters. | 05/12/2025 | 05/12/2025 | – |

### Week 13 Achievements:

* Extended the CDK codebase to define Lambda, CloudFront, and API Gateway resources for the dashboard path end-to-end.
* Designed and documented the integration of Amazon Cognito into the architecture to support authenticated access to the dashboard.
* Successfully deployed and debugged CDK stacks, then merged multiple team stacks into a single deployable application.
* Managed time between project delivery and family responsibilities at the end of the week.
