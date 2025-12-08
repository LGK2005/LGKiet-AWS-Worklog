---
title: "Week 4 Worklog"
date: "2025-09-29"
weight: 04
chapter: false
pre: " <b> 1.4. </b> "
---



### Week 4 Objectives:
- Complete Module 6
- Started on proposal

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   |- Module 6: Database Concept review:<br> &emsp; + Database <br> &emsp; + Session <br> &emsp; + Primary/Foreign Key <br> &emsp; + Index <br> &emsp; + Partitions <br> &emsp; + Execution/Query Plan <br> &emsp; + Log & Buffer <br> &emsp; + RDBMS (Relational Database Manangement System) <br> &emsp; + NOSQL <br> ![Database](/images/1-Worklog/Database.png) <br> &emsp; + OLTP(Online Transaction Processing): For payments, transactions <br> &emsp; + OLAP (Online Analytical Processing): Analyze data, predict trends and patterns <br> - AWS RDS (Relational Database Serive): Include Aurora, MySQL, Postgres SQL , MSSQL, Oracle , Maria <br> &emsp; + Automatic backup <br> &emsp; + Generate read replica <br> &emsp; +Read replica can be turned into primary code <br> &emsp; + Auto Fail Over/Multi AZ (Backups on mutiple AZs) <br> &emsp; + Commonly used for OLTP <br> &emsp; + Encrypt data while at rest/in transit <br> &emsp; + Protected my Security Group and NACL <br> &emsp; + Can change instance size <br> &emsp; + Storage Auto SCaling <br> - Amazon Aurora: Optimized underlying storage infrastructure, uses MySQL and PostgreSQL <br> &emsp; + Back track: revert to previous state <br> &emsp; + Clone <br> &emsp; + Global Database (Multi Region) <br> &emsp; + Multi Master: Many Master Databases <br> - Amazon Redshift: Data warehouse service: PostgreSQL core, optimized for OLAP <br> &emsp; + Uses MMP Database: data is partitioned and saved at computer nodes, a Leader node is used to coordinate and compile queries <br> &emsp; + Stores data in a columnar storage format, useful for OLAP applications <br> ![Columnar](/images/1-Worklog/Columnar.png) <br> &emsp; + Uses SQL and drivers like JDBC and ODBC <br> &emsp; + Provide cost effective services (Transient Cluster/ Redshift spectrum) <br> - Amazon ElastiCache: Creates Cluster Caching Engines (Redis/Memcached) <br> &emsp; + Detects and replaces failed nodes <br> &emsp; + Put before CSDL layer in order to cache data <br> &emsp; + Recommended to use Redis for new workloads <br> &emsp; + Using ElastiCache requires caching logic on applications, not recommended to use default system caching <br> - Formulated a proposal for workshop with teammates <br> - School subject: <br> &emsp; + KS57: Completed Quản trị dữ liệu và an toàn thông tin   | 29/09/2025 | 29/09/2025| [Quản trị dữ liệu và an toàn thông tin](https://www.coursera.org/account/accomplishments/verify/JA236L8TZGD7) |
| 3   |- Lab 43: Guide is broken, the link doesnt go anywhere, going by video <br> &emsp; + Downloaded Schema Conversion Tool <br> &emsp; + Downloaded MSSQL in EC2 Instance <br> &emsp; + No SQL script was given, trying with custom basic MSSQL Database <br> &emsp; + No CloudFormation Stack was given, skipping Oracle Database connection <br> &emsp; + Installed MySQL on EC2 Instance <br> &emsp; + Migrated custom MSSQL Database to MySQL Database using AWS Schema Conversion Tool <br> &emsp; + Created custom RDS to test migration task <br> &emsp; + Attempted to migrate from local machine to RDS <br> &emsp; + Tried to use AWS Replication Agent: Unsuccessful due to it being made for Window/Linux server only, not OS <br> &emsp; + Tried to portforward PC to be used as an endpoint <br> &emsp; + Failed portforwarding, not allowed by ISP   | 30/09/2025 | 30/09/2025      | [Lab 43](https://000043.awsstudygroup.com/) <br><br> [Application Mirgation Service Guide](https://docs.aws.amazon.com/mgn/latest/ug/what-is-application-migration-service.html) |
| 4   |- Found out AWS account's credits are all expired from doing lab 12 <br> - Wrote a support case <br> - Stopping labs for now <br> - Focus on researching about team's proposal| 01/10/2025 | 01/10/2025 <br> - School subject: <br> &emsp; + ENW439c: Completed Research Methodologies | [Research Methodologies](https://www.coursera.org/account/accomplishments/verify/M3368Q4AAMNB) |
| 5   |- Continued doing labs by aquiring help from team member: Created an IAM User with admin privilege for me to log in and use their account <br> - Translate first blog| 02/10/2025 | 02/10/2025      | [Blog 1](content/3-BlogsTranslated/3.1-Blog1/_index.md) |
| 6   |- Joined the AI-Driven Development Life Cycle: Reimagining Software Engineering event <br> - Translated second and third blog| 03/10/2025 | 04/10/2025 | [Blog 2](content/3-BlogsTranslated/3.2-Blog2/_index.md) <br><br> [Blog 3](content/3-BlogsTranslated/3.3-Blog3/_index.md) | 


### Week 4 Achievements:

* Completed a comprehensive review of core database concepts including RDBMS, keys, indexes, partitioning, OLTP/OLAP, and AWS-specific database services.

* Gained theoretical knowledge of the features and use cases for AWS RDS, Amazon Aurora (e.g., Backtrack, Global Database), Amazon Redshift (Data Warehouse for OLAP), and Amazon ElastiCache (caching with Redis/Memcached).

* Database Migration: Attempted a complex database migration lab, demonstrating resourcefulness by:

  *  Sourcing a custom MSSQL Database and installing necessary services on an EC2 instance due to broken lab guides.

  *  Successfully migrating the custom MSSQL database to MySQL using the AWS Schema Conversion Tool (SCT).

* Identified and addressed the issue of expired AWS credits by raising a support case.

* Secured continuation of lab work by setting up an IAM User with admin privileges on a team member's account.

* Formulated a proposal for the team's upcoming workshop with teammates.

* Completed the translation of three blogs.

* Attended the AI-Driven Development Life Cycle: Reimagining Software Engineering event.