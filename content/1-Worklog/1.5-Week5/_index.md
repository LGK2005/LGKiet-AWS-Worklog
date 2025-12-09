---
title: "Week 5 Worklog"
date: "2025-10-06"
weight: 05
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Continue building and planning the workshop proposal.
* Explore workflow automation with n8n and basic Messenger bot development.
* Learn Auto Scaling and monitoring (CloudWatch) for AWS workloads.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | Researched n8n as a workflow automation tool (self-hosted alternative to tools like Zapier). <br> Learned about core concepts: workflows, triggers, nodes, credentials, and executions. <br> Deployed n8n locally using Docker (basic docker-compose setup) and created simple test workflows (HTTP trigger, timers, basic data transformation). | 06/10/2025 | 06/10/2025 | [n8n Documentation](https://docs.n8n.io/) |
| Tue | Followed the tutorial video to build a Messenger bot using n8n and Facebook: <br> &emsp; + Set up a Facebook app and page for the bot. <br> &emsp; + Configured webhook and tokens so that messages from Messenger are sent to n8n. <br> &emsp; + Built a simple reply flow in n8n to handle incoming messages and send responses back to Messenger. <br> &emsp; + Tested the bot end-to-end from Messenger on phone/desktop. | 07/10/2025 | 07/10/2025 | [Messenger bot tutorial](https://youtu.be/ieFnSN2Gvs4) |
| Wed | Translated three assigned blog posts for the workshop materials and documentation, focusing on accuracy of technical terminology and style. | 08/10/2025 | 08/10/2025 | [Blog1](https://docs.google.com/document/d/1ge1HDyGWudRqSg6LUp2y-jgBTH0y7eFBcZLS2q93OK8/edit?usp=sharing) <br> [Blog2](https://docs.google.com/document/d/11Nvdi9t93mp6l4Trngs8DZFN1Cj6UJLeBS4FChl-Hic/edit?usp=sharing) <br> [Blog3](https://docs.google.com/document/d/1CmH2ggwue55AxPlKajLlMa3d5sBV_1vgqRxyIRLuD_E/edit?usp=sharing) |
| Thu | Auto Scaling workshop – “FCJ Management with Auto Scaling Group”: <br> &emsp; + Reviewed the overall architecture and prerequisites for deploying the FCJ Management application. <br> &emsp; + Studied how to create a Launch Template based on an existing FCJ Management AMI. <br> &emsp; + Learned how to configure an Elastic Load Balancer to distribute traffic across application instances. <br> &emsp; + Learned the steps to create and configure an Auto Scaling Group, scaling policies, and how to test scaling and failover. | 09/10/2025 | 09/10/2025 | [FCJ Management with Auto Scaling Group](https://000006.awsstudygroup.com/vi/) |
| Fri | AWS CloudWatch Workshop: <br> &emsp; + Studied the overall purpose of Amazon CloudWatch for monitoring metrics, logs, and events across AWS resources and applications. <br> &emsp; + Reviewed preparatory steps for enabling CloudWatch in an account. <br> &emsp; + Learned how CloudWatch Metrics work and what kinds of default and custom metrics can be collected. <br> &emsp; + Studied CloudWatch Logs and how applications can send logs for centralized analysis. <br> &emsp; + Learned how to configure CloudWatch Alarms and Dashboards, and noted cleanup steps for lab resources. | 10/10/2025 | 10/10/2025 | [AWS CloudWatch Workshop](https://000008.awsstudygroup.com/vi/) |

### Week 5 Achievements:

* Learned the basics of n8n, deployed it with Docker, and successfully connected it to Facebook Messenger to build a simple bot. [web:0]
* Translated three technical blogs, improving both technical understanding and written communication for the workshop. [web:0]
* Gained theoretical understanding of deploying the FCJ Management application using Launch Templates, Load Balancer, and Auto Scaling Group on AWS. [web:1]
* Built foundational knowledge of Amazon CloudWatch, including metrics, logs, alarms, and dashboards for monitoring AWS workloads. [web:1]
