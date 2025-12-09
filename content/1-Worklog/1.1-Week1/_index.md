---
title: "Week 1 Worklog"
date: "2025-09-09"
weight: 01
chapter: false
pre: " <b> 1.1. </b> "
---

### Week 1 Objectives:

* Connect with FCJ members and mentors.
* Find out what working in an office is like.
* Learn the basics of AWS, console and CLI.
* Complete first module.
* Learn how to use AWS pricing calculator.


### Tasks to be carried out this week:
| Day |Task| Start Date | Completion Date | Reference Material |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| Mon   | - Read internship rules <br> - Create AWS account <br>  - Learnt what AWS is <br>- Module 1 Lab 1 Done (Learnt how to create AWS account and manage user groups) <br>- Module 1 Lab 7 Done (Learnt how to create budgets of using the service) <br>- Lab 7-3 (Usage Budget) cannot be done, error in usage type dropdown, showing nothing <br>- Module 1 Lab 9 Done (Learnt about AWS Support Services, Its type, benefits and how to request supports) | 08/09/2025 | 08/09/2025 | [Create new AWS Account](https://000001.awsstudygroup.com/1-create-new-aws-account/) <br><br> [MFA for AWS Accounts](https://000001.awsstudygroup.com/2-mfa-setup-for-aws-user-root/) <br><br> [Create Admin Group and Admin User](https://000001.awsstudygroup.com/3-create-admin-user-and-group/) <br><br> [Account Authentication Support](https://000001.awsstudygroup.com/4-verify-new-account/) <br><br> [Explore and Configure AWS Management Console](https://000001.awsstudygroup.com/5-explore-and-configure-the-aws-management-console/) <br><br> [Creating Support Cases and Case Management in AWS](https://000001.awsstudygroup.com/6-support-cases/) |
| Tue   | - Get started on Module 2 theory: <br>&emsp; + Learnt about VPC (Amazon Virtual Private Cloud)<br>&emsp; + Learnt about Subnets and Routetable, Security Groups<br>&emsp; + Learnt about ENI and EIP<br>&emsp; + Learnt about VPC Peering and Transit Gateway <br>&emsp; + Learnt about Elastic Load Balancing<br>&emsp; + Learnt about EC2<br> - Setup site for workshop report <br> - Installed Hugo <br> - Successfully write worklog using markdown and Hugo | 09/09/2025 | 09/09/2025 | <https://cloudjourney.awsstudygroup.com/> |
| Wed   | - Complete Module 2's labs <br> - Lab 3: <br>&emsp; + Learnt about the resources necessary to create and run EC2 instances <br>&emsp; + Successfully configured and run EC2 instances <br>&emsp; + Successfully connect and ping to EC2 instances <br>&emsp; + Created NAT Gateway to allow private EC2 connections <br> - Lab 10: <br>&emsp; + Learnt how to create and use keypair for security <br>&emsp; + Learnt how to configure security group to manage connections <br>&emsp; + Successfully connect and use RDP via EC2 <br>&emsp; + Set up hybrid DNS with Route 53 Resolver (In progress, Cloud Formation template didn't create security group to proceed with the lab) <br> - Lab 19: <br>&emsp; + Successfully created VPC Peering Connection <br> &emsp; + Learnt how to configure Network ACLs <br> &emsp; + Enabled Cross-Peer DNS to resolve private host names <br> - Downloaded and used MobaXTerm to connect to EC2 instances <br> - Downloaded and used Putty to configures keypairs    | 10/09/2025 | 11/09/2025 | [Lab 3](https://000003.awsstudygroup.com/) <br><br> [Lab 10](https://000010.awsstudygroup.com/) <br><br> [Lab 19](https://000019.awsstudygroup.com/) |
| Thu   | - Lab 20: <br>&emsp; + Successfully created AWS Transit Gateway to allow for connection between VPCs via a common hub <br>&emsp;&emsp; • The Cloud Formation template yaml file isnt up to date, creation failed <br>&emsp;&emsp; • Fixed template file, changed EC2 instance type to t3.micro <br> - Learnt the hard way why you need to clean up resources after a lab, got charged 12$ credits <br> - Verified the cost and budget plans worked as intended, notified over email. | 11/09/2025 | 11/09/2025 | [Lab 20](https://000020.awsstudygroup.com/) |
| Fri   | - Get started on Module 3 theory <br>&emsp; + Learnt about EBS, Instance store feature and check User and Meta Data <br>&emsp; + Learnt about Amazon Lightsail <br>&emsp; + Learnt about Elastic File System(EFS) and FSx <br>&emsp; + Learnt about MGN <br>&emsp; + Learnt how to use S3 Bucket on AWS <br>- Complete Module 3's labs <br>- Lab 13: Successuly created Backup Plan and Vaults for data in S3 Bucket <br>&emsp; + Successfully setted up notification for Backup events <br>&emsp; + Successfully restored backup <br> - Lab 24: <br>&emsp; + Created storage gateway <br>&emsp; + Succesfully completed file sharing <br> - Lab 57: <br>&emsp; + Successfully hosted static website using S3 Bucket <br>&emsp; + Successfully configured access modifiers <br>&emsp; + Accelerate Static Websites with Cloudfront configuration did not work, skipping this step <br>&emsp; + Successfully created bucket versions <br>&emsp; + Moved objects between buckets <br>&emsp; + Replicated bucket across regions.| 12/09/2025 | 12/09/2025 | [Lab 13](https://000013.awsstudygroup.com/) <br><br> [Lab 24](https://000024.awsstudygroup.com/) <br><br> [Lab 57](https://000057.awsstudygroup.com/)|


### Week 1 Achievements:

* Created and secured an AWS account, including setting up budgets and exploring support services.

* Completed theory and practical labs for VPC, Subnets, Security Groups, and Routetables.

* Successfully deployed and connected to EC2 instances, configured a NAT Gateway, and managed key connections using VPC Peering and AWS Transit Gateway.

* Gained hands-on experience with S3 Buckets (static website hosting, versioning, replication), AWS Backup, and Storage Gateway.

* Documentation Setup: Successfully installed Hugo and configured the site for writing the worklog using markdown.

* Tool Proficiency: Learned to use MobaXTerm and PuTTY for connecting to and managing EC2 instances.

* Successfully fixed an outdated CloudFormation template during the Transit Gateway lab and learned cost management through budget alerts.

* Completed Module 1 and Module 2, and made a strong start on Module 3.
