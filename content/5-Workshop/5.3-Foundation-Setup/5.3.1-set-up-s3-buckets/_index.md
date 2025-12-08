---
title : "Set up S3 buckets"
date: "2000-01-01"
weight : 01
chapter : false
pre : " <b> 5.3.1. </b> "
---

In this section, you will create 5 S3 buckets that serve as the foundation for the Auto Incident Response system.

**Important**: Replace `ACCOUNT_ID` with your AWS Account ID and `REGION` with your target region (e.g., us-east-1) in all bucket names.

### Bucket Names

1. **incident-response-log-list-bucket-ACCOUNT_ID-REGION** - Primary log collection bucket
2. **processed-cloudtrail-logs-ACCOUNT_ID-REGION** - Stores processed CloudTrail logs
3. **athena-query-results-ACCOUNT_ID-REGION** - Stores Athena query results
4. **processed-cloudwatch-logs-ACCOUNT_ID-REGION** - Stores processed CloudWatch logs
5. **processed-guardduty-findings-ACCOUNT_ID-REGION** - Stores processed GuardDuty findings

### Bucket Creation Instructions

1. **Open the Amazon S3 Console**
   - Navigate to https://console.aws.amazon.com/s3/
   - Or: AWS Management Console → Services → S3

   > [Screenshot: AWS S3 Console Homepage]

2. **Click on "Create bucket"**

   > [Screenshot: Create Bucket Button]

3. **General configuration**:
   - **Bucket name**: Enter `incident-response-log-list-bucket-ACCOUNT_ID-REGION`
     - Example: `incident-response-log-list-bucket-123456789012-us-east-1`
   - **AWS Region**: Select your target region (e.g., US East (N. Virginia) us-east-1)

   > [Screenshot: S3 Bucket Configuration - Bucket name and region]

4. **Object Ownership**:
   - Keep default: **ACLs disabled (recommended)**

   > [Screenshot: S3 Bucket Configuration - Object Ownership]

5. **Block Public Access settings for this bucket**:
   - Check **"Block all public access"**
   - Ensure all 4 sub-options are checked:
     - ✓ Block public access to buckets and objects granted through new access control lists (ACLs)
     - ✓ Block public access to buckets and objects granted through any access control lists (ACLs)
     - ✓ Block public access to buckets and objects granted through new public bucket or access point policies
     - ✓ Block public and cross-account access to buckets and objects through any public bucket or access point policies

   > [Screenshot: S3 Bucket Configuration - Block Public Access settings]

6. **Bucket Versioning**:
   - Select **"Enable"**

   > [Screenshot: Bucket Versioning Setting]

7. **Tags** (optional):
   - Add tags if desired
   - Example: Key=`Purpose`, Value=`IncidentResponse`

8. **Default encryption**:
   - **Encryption type**: Select **"Server-side encryption with Amazon S3 managed keys (SSE-S3)"**
   - **Bucket Key**: Keep default (**Enabled**)

   > [Screenshot: Default Encryption Settings]

9. **Advanced settings**:
   - Keep all defaults

10. **Click "Create bucket"**

    > [Screenshot: Create bucket button at bottom of page]

11. **Verify bucket creation**:
    - You should see a success message
    - The bucket should appear in your S3 buckets list

    > [Screenshot: S3 Console showing newly created bucket]

12. **Repeat steps 2-10 for the remaining 4 buckets**:
    - `processed-cloudtrail-logs-ACCOUNT_ID-REGION`
    - `athena-query-results-ACCOUNT_ID-REGION`
    - `processed-cloudwatch-logs-ACCOUNT_ID-REGION`
    - `processed-guardduty-findings-ACCOUNT_ID-REGION`

13. **Verify all 5 buckets are created**:
    - Navigate to S3 Console
    - You should see all 5 buckets listed

    > [Screenshot: S3 Console showing all 5 buckets]
