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
* Learn how to use AWS pricing calculator.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | - Read internship rules <br> - Create AWS account <br> - Learnt what AWS is <br> - Module 1 Lab 1 Done (Learnt how to create AWS account and manage user groups) <br> - Module 1 Lab 7 Done (Learnt how to create budgets of using the service) <br> - Lab 7-3 (Usage Budget) cannot be done, error in usage type dropdown, showing nothing <br> - Module 1 Lab 9 Done (Learnt about AWS Support Services, its type, benefits and how to request supports) | 08/09/2025 | 08/09/2025 | [Create new AWS Account](https://000001.awsstudygroup.com/1-create-new-aws-account/) <br><br> [MFA for AWS Accounts](https://000001.awsstudygroup.com/2-mfa-setup-for-aws-user-root/) <br><br> [Create Admin Group and Admin User](https://000001.awsstudygroup.com/3-create-admin-user-and-group/) <br><br> [Account Authentication Support](https://000001.awsstudygroup.com/4-verify-new-account/) <br><br> [Explore and Configure AWS Management Console](https://000001.awsstudygroup.com/5-explore-and-configure-the-aws-management-console/) <br><br> [Creating Support Cases and Case Management in AWS](https://000001.awsstudygroup.com/6-support-cases/) |
| Tue | - Learnt overview of AWS Budgets and why cost management is important on AWS <br> - Studied 4 main budget types: Cost Budget, Usage Budget, RI Budget, Savings Plans Budget <br> - Practised creating Cost Budget to receive alerts when total cost exceeds thresholds <br> - Practised creating Usage Budget to monitor usage of specific services such as EC2 hours <br> - Learnt the difference between RI Budget and Savings Plans Budget and how they help reduce long‑term instance costs <br> - Reviewed best practices for setting alert thresholds and notification recipients | 09/09/2025 | 09/09/2025 | [Bắt đầu với AWS Budget](https://000007.awsstudygroup.com/vi/) <br><br> [Tạo Cost Budget](https://000007.awsstudygroup.com/vi/1-cost-budgets/) <br><br> [Tạo Usage Budget](https://000007.awsstudygroup.com/vi/2-usage-budget/) <br><br> [Tạo RI Budget](https://000007.awsstudygroup.com/vi/3-reservation-budget/) <br><br> [Tạo Savings Plans Budget](https://000007.awsstudygroup.com/vi/4-saving-plans-budget/) |
| Wed | - Learnt overview of AWS Identity and Access Management (IAM) <br> - Studied core IAM concepts: IAM Users, IAM Groups, IAM Roles and IAM Policies <br> - Practised creating IAM Groups and IAM Users for admin access <br> - Attached and tested IAM Policies following the principle of least privilege <br> - Created IAM Roles to provide temporary access instead of attaching long‑term credentials <br> - Practised switching roles between accounts and verifying access <br> - Reviewed IAM security best practices such as using CloudTrail to monitor access and cleaning up unused users/roles | 10/09/2025 | 10/09/2025 | [Giới thiệu IAM](https://000002.awsstudygroup.com/vi/1-introduction/) <br><br> [Tạo IAM Group và IAM User](https://000002.awsstudygroup.com/vi/2-create-admin-user-and-group/) <br><br> [Tạo IAM Role](https://000002.awsstudygroup.com/vi/3-aws-role/) <br><br> [Chuyển đổi IAM Role](https://000002.awsstudygroup.com/vi/4-switch-roles/) |
| Thu | - Learnt overview of Amazon Virtual Private Cloud (VPC) and its role as the core networking service on AWS <br> - Studied VPC components: subnets, route tables, internet gateway, NAT gateway and VPN gateway <br> - Learnt about network security in VPC using Security Groups and Network ACLs <br> - Practised deploying Amazon EC2 instances inside public and private subnets <br> - Studied and practised setting up AWS Site-to-Site VPN between on-premises network and AWS VPC <br> - Reviewed best practices for segmenting networks and monitoring traffic with VPC Flow Logs | 11/09/2025 | 11/09/2025 | [Giới thiệu Amazon VPC](https://000003.awsstudygroup.com/vi/1-introduce/) <br><br> [Network Security với Security Groups và NACLs](https://000003.awsstudygroup.com/vi/2-firewallinvpc/) <br><br> [Triển khai Amazon EC2 Instance](https://000003.awsstudygroup.com/vi/4-createec2server/) <br><br> [Thiết lập AWS Site-to-Site VPN](https://000003.awsstudygroup.com/vi/5-vpnsitetosite/) |
| Fri | - Learnt overview of Amazon Elastic Compute Cloud (Amazon EC2) and its use cases <br> - Reviewed prerequisites for working with EC2: key pairs, security groups and networking setup <br> - Practised launching Windows EC2 instance and connecting via RDP <br> - Practised launching Linux EC2 instance and connecting via SSH <br> - Explored basic EC2 operations: start, stop, reboot, resize instance type and manage volumes <br> - Deployed sample AWS User Management application on Linux and Windows EC2 instances using Node.js <br> - Reviewed cost and usage governance for EC2 and cleaned up resources after the lab | 12/09/2025 | 12/09/2025 | [Giới thiệu Amazon EC2](https://000004.awsstudygroup.com/vi/1-introduce/) <br><br> [Các bước chuẩn bị](https://000004.awsstudygroup.com/vi/2-prerequiste/) <br><br> [Khởi tạo Windows instance](https://000004.awsstudygroup.com/vi/3-launchwindowsinstance/) <br><br> [Khởi tạo Linux instance](https://000004.awsstudygroup.com/vi/4-launchlinuxinstance/) <br><br> [Amazon EC2 cơ bản](https://000004.awsstudygroup.com/vi/5-amazonec2basic/) <br><br> [Triển khai ứng dụng Node.js trên EC2](https://000004.awsstudygroup.com/vi/6-awsfcjmanagement-linux/) |

### Week 1 Achievements:

* Built and secured a new AWS account, including MFA, admin IAM users and basic support case handling.
* Gained a solid foundation in AWS cost management by creating and testing multiple AWS Budgets with email alerts.
* Strengthened security and access governance using IAM users, groups, roles and least‑privilege policies.
* Designed and operated networking on AWS with Amazon VPC, including public/private subnets, security groups, NACLs and Site‑to‑Site VPN.
* Deployed and managed Windows and Linux EC2 instances, including connecting via RDP/SSH and performing common EC2 operations.
* Successfully deployed a sample AWS User Management application on EC2 and practised cleaning up resources to control costs.
