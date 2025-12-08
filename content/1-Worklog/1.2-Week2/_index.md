---
title: "Week 2 Worklog"
date: "2025-09-15"
weight: 02
chapter: false
pre: " <b> 1.2. </b> "
---


### Week 2 Objectives:

* Complete Module 3 & 4
* Help team members get up to speed
* Discuss workshop ideas
* Do first optional research: AWS Well Architected Framework
* Check out AWS Advanced Networking - Specialty Study Guide
* Check out AWS Microsoft Workload
* Check out AWS Skill Builder

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   |- Started on module 4 <br> - Learnt about AWS Storage services <br> - Learnt about S3 Bucket Accesspoint and Storage class. | 15/09/2025 | 15/09/2025      |
| 3   |- Lab 4 <br> - Lab 6: RDS Database <br> -Succesfully used Linux via EC2 instance to: <br>&emsp; + Install and use MySQL database <br>&emsp; + Install and run a web apllication, can be connected to from browers <br> - Created Load Balancer and Target Groups <br> -Paessler Webstress tool has been discontinued, cannot test using the given tool  <br> - Successfully install Siege on EC2 instance to load test: <br>&emsp; + Ran loadtest, simulated 50 users at the same time for 10 minutes <br>&emsp; + The EC2 instance is terminated and load balanced 10 times in succession <br>&emsp; + Seige automatically stopped after 5 minutes due to too much packet loss <br>&emsp; + The reason might be due to the EC2 instance and RDS Database were created using the only available free tier options, and could not handle the increased traffics.    <br> - Succesfully hosted database using RDS| 16/09/2025 | 16/09/2025      | [Lab 6](https://000006.awsstudygroup.com/) |
| 4   | <br> Module 4 3, module 4 4 <br> - Help teammate with Lab 5 <br> - Instruction of lab 5 is missing some steps: 5.5.3: The given script didnt connect the RDS database to MySQL, 5.5.5: The instruction is missing the step: cd to the application folder| 17/09/2025 | 17/09/2025      | [Lab 5](https://000005.awsstudygroup.com/) |
| 5   | - Joined the AWS Cloud Day 2025 HCM Event: Gen AI and Data track <br>&emsp; | 18/09/2025 | 18/09/2025      | [Vietnam Cloud Day Agenda](https://pages.awscloud.com/vietnam-cloud-day-hcmc-connect-edition.html?trk=a3b21210-a497-4502-8073-982920d98210&sc_channel=em#agenda) <br><br> [Event Summary and Experience](../../4-EventParticipated/4.1-Event1/)  |
| 6   | - Retry Lab 10: <br>&emsp; + Fix given template: Region changed to ap-southeast-1, instance changed to t3.micro <br>&emsp; + Successfully configured endpoints and rules in Route 53 for hybrid DNS <br>&emsp; + Succesfully deployed Microsoft AD<br> - Lab 8:<br>&emsp; + Viewed metrics and graph using Cloudwatch on selected EC2 Instances <br>&emsp; + Learnt the basics on monitoring logs <br>&emsp; + 8.4.2: cannot be done: Cant find the resource s3://workshop-template-bucket/logger.py . <br>&emsp; + Configured Cloudwatch Alarm and Dashboard  <br> - Lab 14: Installing Ubuntu <br> - Reformatted worklog <br> - Wrote about Cloud Day 2025 experiences <br><br> - Additional research on AWS Well Architected Framework: <br>&emsp; + Documents a set of foundational questions that enable you to understand how a specific architecture aligns with cloud best practices <br>&emsp; + The pillars: <br>&emsp;&emsp; • Operational Excellence: Focuses on running and monitoring systems to deliver business value, and continually improving supporting processes and procedures. <br>&emsp;&emsp; • Security: Focuses on protecting information, systems, and assets, while delivering business value through risk assessments and mitigation strategies. <br>&emsp;&emsp; • Reliability: Focuses on the ability of a workload to perform its intended function correctly and consistently when it's expected to. <br>&emsp;&emsp; • Performance Efficiency: Focuses on using computing resources efficiently to meet system requirements, and maintaining that efficiency as demand changes. <br>&emsp;&emsp; • Cost Optimization: Focuses on avoiding unnecessary costs by managing and controlling where money is spent in the cloud. <br>&emsp;&emsp; • ustainability: Focuses on minimizing the environmental impacts of running cloud workloads. <br>&emsp; **+ Purpose:** <br>&emsp;&emsp; • A cloud service for reviewing and measuring your workloads against AWS best practices to build more secure, resilient, high-performing, and cost-effective systems.<br>&emsp;&emsp; • Core Function: Identifies High Risk Issues (HRIs) and Medium Risk Issues (MRIs) in your architecture and provides an improvement plan to mitigate them. <br>&emsp; **+ Usage:** <br>&emsp;&emsp; • Step 1: Define a Workload: Specify the name, environment, owner, and regions for the application or system you are reviewing.<br>&emsp;&emsp; • Step 2: Document the State: Answer questions based on the pillars of the AWS Well-Architected Framework (Security, Reliability, etc.) and save a "milestone" to capture your progress.<br>&emsp;&emsp; • Step 3: Review the Improvement Plan: The tool generates a prioritized list of risks (HRIs and MRIs) based on your answers.<br>&emsp;&emsp; • Step 4: Make Improvements & Measure: Update your architecture based on the plan, then update your answers in the tool to track the reduction in risks over time <br>&emsp; **+ Key Features:** <br>&emsp;&emsp; • Workloads: The central component representing your application; can be viewed, edited, shared, and deleted. <br>&emsp;&emsp; • Lenses: Provide focused questions for specific technologies (e.g., Serverless Lens) or industries. You can also create Custom Lenses for internal standards. <br>&emsp;&emsp; • Review Templates & Profiles: Help standardize reviews by pre-filling common answers (Templates) and prioritizing questions based on business goals (Profiles). <br>&emsp;&emsp; • Jira Integration: Allows you to sync improvement items directly from the Well-Architected Tool into your Jira projects as epics, tasks, and sub-tasks for streamlined tracking. <br>&emsp; **+ Security:** <br>&emsp;&emsp; • Shared Responsibility Model: AWS secures the cloud infrastructure, while you are responsible for securing your workloads in the cloud. <br>&emsp;&emsp; • IAM Integration: Access is controlled through AWS IAM, with pre-built policies for full access and read-only access. <br>&emsp;&emsp; • Data Protection: Recommends using IAM users (not root), enabling MFA, and avoiding placing sensitive data in free-form text fields. <br>&emsp;&emsp; • Monitoring & Auditing: Integrates with AWS CloudTrail to log all API activity and with Amazon EventBridge to trigger automated notifications.  | 19/09/2025 | 20/09/2025      | [Lab 10](https://000010.awsstudygroup.com/) <br><br> [Lab 8](https://000008.awsstudygroup.com/) <br><br> [Lab 14](https://000014.awsstudygroup.com/) <br><br> [AWS Well Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/userguide/intro.html) |


### Week 2 Achievements:

* Successfully completed core labs in Module 3 (focused on RDS, Load Balancing) and made significant progress in Module 4.

* Utilized Linux via EC2 to install and run a MySQL database and deploy a connected web application, successfully hosting the database using RDS (Relational Database Service).

* Successfully set up a Load Balancer and Target Groups, and adapted to a discontinued tool by installing and using Siege on an EC2 instance to execute a load test.

* Configured Route 53 endpoints and rules for a hybrid DNS setup, including the deployment of Microsoft AD.

* Gained foundational knowledge in cloud monitoring by viewing metrics and graphs in CloudWatch, configuring Alarms and Dashboards, and learning the basics of log management.

* Team Support: Assisted team members with labs, identifying and helping to correct missing steps in the lab instructions.

* Attended the AWS Cloud Day 2025 HCM Event (Gen AI and Data track).

* Optional Research: Completed research on the AWS Well-Architected Framework, documenting its purpose, usage steps, key features (Lenses, Templates, Profiles), and security considerations.

* Technical Setup: Successfully installed Ubuntu on a machine and reformatted the worklog for improved presentation.