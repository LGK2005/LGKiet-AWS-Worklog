---
title: "Workshop"
date: "2000-01-01"
weight: 05
chapter: false
pre: " <b> 5. </b> "
---

# AWS Auto Incident Response System Setup

#### Overview

This guide provides a complete, step-by-step procedure for deploying our automated incident response and forensic system in AWS. This system leverages **CloudTrail**, **GuardDuty**, **VPC Flow Logs**, **Kinesis Firehose**, **Glue**, **Athena**, and **Lambda** functions orchestrated by **AWS Step Functions** to automatically detect, analyze, and quarantine compromised resources like EC2 instances and IAM users.



#### Content
1. [Overview](5.1-Workshop-overview/)
1. [Prerequisites](5.2-Prerequisites)
2. [Phase 1: Foundation Setup](5.3-Foundation-Setup/)
3. [Phase 2: Monitoring Setup](5.4-Monitoring-Setup/)
4. [Phase 3: Processing Setup](5.5-Processing-Setup/)
5. [Phase 4: Automation Setup](5.6-Automation-Setup/)
6. [Phase 5: Dashboard Setup](5.7-Dashboard-Setup/)
7. [Use CDK](5.8-Use-CDK/)
8. [Cleanup](5.9-Cleanup/)