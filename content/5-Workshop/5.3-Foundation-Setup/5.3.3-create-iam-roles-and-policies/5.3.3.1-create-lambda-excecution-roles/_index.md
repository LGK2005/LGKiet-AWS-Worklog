---
title : "Create Lambda Execution Roles"
date: "2000-01-01"
weight : 01
chapter : false
pre : " <b> 5.3.3.1. </b> "
---


#### Create CloudTrailETLLambdaServiceRole

1.  **Open the IAM Console**:

      - Navigate to https://console.aws.amazon.com/iam/
      - Or: AWS Management Console → Search for "IAM" → Click "IAM"

    > [Screenshot: IAM Console Dashboard]

2.  **Navigate to Roles**:

      - In the left sidebar, click **"Roles"**

    > [Screenshot: IAM Console with Roles menu item]

3.  **Click "Create role"**

    > [Screenshot: IAM Roles page with Create role button]

4.  **Select trusted entity**:

      - **Trusted entity type**: Select **"AWS service"**
      - **Use case**: Select **"Lambda"**
      - Click **"Next"**

    > [Screenshot: IAM Create role - Select trusted entity]

5.  **Add permissions**:

      - In the search box, type `AWSLambdaBasicExecutionRole`
      - Check the box next to **"AWSLambdaBasicExecutionRole"**
      - Click **"Next"**

    > [Screenshot: IAM Create role - Add permissions]

6.  **Name, review, and create**:

      - **Role name**: Enter `CloudTrailETLLambdaServiceRole`
      - **Description**: Enter `Execution role for CloudTrail ETL Lambda function`
      - Click **"Create role"**

    > [Screenshot: IAM Create role - Name and review]

7.  **Add inline policy**:

      - After creation, you'll be on the role details page
      - Click on the **"Permissions"** tab
      - Click **"Add permissions"** → **"Create inline policy"**

    > [Screenshot: IAM Role permissions tab with Add permissions button]

8.  **Create inline policy**:

      - Click on the **"JSON"** tab
      - Paste the following policy (replace `ACCOUNT_ID` and `REGION`):

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "firehose:PutRecord",
        "firehose:PutRecordBatch"
      ],
      "Resource": "arn:aws:firehose:REGION:ACCOUNT_ID:deliverystream/cloudtrail-firehose-stream"
    }
  ]
}
```

> [Screenshot: IAM Create policy - JSON editor]

9.  **Click "Next"**

10. **Policy name**:

      - **Policy name**: Enter `CloudTrailETLPolicy`
      - Click **"Create policy"**

    > [Screenshot: IAM Create policy - Review and create]

11. **Verify role creation**:

      - You should see the role with both managed and inline policies attached

    > [Screenshot: IAM Role showing AWSLambdaBasicExecutionRole and CloudTrailETLPolicy]

#### Create Remaining Lambda Roles

**Follow the same process for each role below** (steps 3-11):

**GuardDutyETLLambdaServiceRole**

  - Role name: `GuardDutyETLLambdaServiceRole`
  - Description: `Execution role for GuardDuty ETL Lambda function`
  - Managed policy: AWSLambdaBasicExecutionRole
  - Inline policy name: `GuardDutyETLPolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/*",
        "arn:aws:s3:::processed-guardduty-findings-ACCOUNT_ID-REGION/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "kms:Decrypt",
      "Resource": "arn:aws:kms:REGION:ACCOUNT_ID:key/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "glue:CreatePartition",
        "glue:GetPartition"
      ],
      "Resource": [
        "arn:aws:glue:REGION:ACCOUNT_ID:catalog",
        "arn:aws:glue:REGION:ACCOUNT_ID:database/security_logs",
        "arn:aws:glue:REGION:ACCOUNT_ID:table/security_logs/processed_guardduty"
      ]
    }
  ]
}
```

**CloudWatchETLLambdaServiceRole**

  - Role name: `CloudWatchETLLambdaServiceRole`
  - Description: `Execution role for VPC DNS logs ETL Lambda`
  - Managed policy: AWSLambdaBasicExecutionRole
  - Inline policy name: `CloudWatchETLPolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "firehose:PutRecord",
        "firehose:PutRecordBatch"
      ],
      "Resource": "arn:aws:firehose:REGION:ACCOUNT_ID:deliverystream/vpc-dns-firehose-stream"
    }
  ]
}
```

**CloudWatchENIETLLambdaServiceRole**

  - Role name: `CloudWatchENIETLLambdaServiceRole`
  - Description: `Execution role for VPC Flow logs ETL Lambda`
  - Managed policy: AWSLambdaBasicExecutionRole
  - Inline policy name: `CloudWatchENIETLPolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "firehose:PutRecord",
        "firehose:PutRecordBatch"
      ],
      "Resource": "arn:aws:firehose:REGION:ACCOUNT_ID:deliverystream/vpc-flow-firehose-stream"
    }
  ]
}
```

**CloudWatchExportLambdaServiceRole**

  - Role name: `CloudWatchExportLambdaServiceRole`
  - Description: `Execution role for CloudWatch log export Lambda`
  - Managed policy: AWSLambdaBasicExecutionRole
  - Inline policy name: `CloudWatchExportPolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateExportTask",
        "logs:DescribeExportTasks",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:logs:REGION:ACCOUNT_ID:log-group:*",
        "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/*"
      ]
    }
  ]
}
```

**ParseFindingsLambdaServiceRole**

  - Role name: `ParseFindingsLambdaServiceRole`
  - Description: `Execution role for parsing GuardDuty findings`
  - Managed policy: AWSLambdaBasicExecutionRole
  - **No inline policy needed**

**IsolateEC2LambdaServiceRole**

  - Role name: `IsolateEC2LambdaServiceRole`
  - Description: `Execution role for isolating compromised EC2 instances`
  - Managed policy: AWSLambdaBasicExecutionRole
  - Inline policy name: `IsolateEC2Policy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ec2:ModifyInstanceAttribute"
      ],
      "Resource": "*"
    }
  ]
}
```

**QuarantineIAMLambdaServiceRole**

  - Role name: `QuarantineIAMLambdaServiceRole`
  - Description: `Execution role for quarantining compromised IAM users`
  - Managed policy: AWSLambdaBasicExecutionRole
  - Inline policy name: `QuarantineIAMPolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iam:AttachUserPolicy",
        "iam:ListAttachedUserPolicies"
      ],
      "Resource": [
        "arn:aws:iam::ACCOUNT_ID:user/*",
        "arn:aws:iam::ACCOUNT_ID:policy/IrQuarantineIAMPolicy"
      ]
    }
  ]
}
```

**AlertDispatchLambdaServiceRole**

  - Role name: `AlertDispatchLambdaServiceRole`
  - Description: `Execution role for dispatching alerts via SNS/SES/Slack`
  - Managed policy: AWSLambdaBasicExecutionRole
  - Inline policy name: `AlertDispatchPolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sns:Publish",
      "Resource": "arn:aws:sns:REGION:ACCOUNT_ID:IncidentResponseAlerts"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ses:SendEmail",
        "ses:SendRawEmail"
      ],
      "Resource": "*"
    }
  ]
}
```
### 3.2 Create Firehose Roles

#### 3.2.1 Create CloudTrailFirehoseRole

1.  **Open IAM Console** → **Roles** → **Create role**

2.  **Select trusted entity**:

      - **Trusted entity type**: **AWS service**
      - **Use case**: Select **"Kinesis"** → **"Kinesis Firehose"**
      - Click **"Next"**

    > [Screenshot: IAM Create role - Select Kinesis Firehose trusted entity]

3.  **Add permissions**:

      - Skip adding managed policies (we'll add inline policy)
      - Click **"Next"**

4.  **Name and create**:

      - **Role name**: `CloudTrailFirehoseRole`
      - **Description**: `Allows Firehose to write CloudTrail logs to S3`
      - Click **"Create role"**

5.  **Add inline policy**:

      - Policy name: `FirehosePolicy`
      - Policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetBucketLocation",
        "s3:ListBucket",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::processed-cloudtrail-logs-ACCOUNT_ID-REGION",
        "arn:aws:s3:::processed-cloudtrail-logs-ACCOUNT_ID-REGION/*"
      ]
    }
  ]
}
```

#### 3.2.2 Create CloudWatchFirehoseRole

  - Role name: `CloudWatchFirehoseRole`
  - Description: `Allows Firehose to write CloudWatch logs to S3`
  - Trusted entity: Kinesis Firehose
  - Inline policy name: `FirehosePolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetBucketLocation",
        "s3:ListBucket",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::processed-cloudwatch-logs-ACCOUNT_ID-REGION",
        "arn:aws:s3:::processed-cloudwatch-logs-ACCOUNT_ID-REGION/*"
      ]
    }
  ]
}
```

### 3.3 Create Step Functions Role

#### 3.3.1 Create StepFunctionsRole

1.  **Create role**:

      - **Trusted entity**: Step Functions
      - **Role name**: `StepFunctionsRole`
      - **Description**: `Execution role for Incident Response Step Functions`

2.  **Add TWO inline policies**:

**Policy 1: LambdaInvokePolicy**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "lambda:InvokeFunction",
      "Resource": [
        "arn:aws:lambda:REGION:ACCOUNT_ID:function:ir-isolate-ec2-lambda",
        "arn:aws:lambda:REGION:ACCOUNT_ID:function:ir-parse-findings-lambda",
        "arn:aws:lambda:REGION:ACCOUNT_ID:function:ir-quarantine-iam-lambda"
      ]
    }
  ]
}
```

**Policy 2: EC2AutoScalingPolicy**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "autoscaling:DescribeAutoScalingInstances",
        "autoscaling:DetachInstances",
        "autoscaling:UpdateAutoScalingGroup",
        "ec2:CreateSnapshot",
        "ec2:CreateTags",
        "ec2:DescribeVolumes",
        "ec2:ModifyInstanceAttribute"
      ],
      "Resource": "*"
    }
  ]
}
```

### 3.4 Create EventBridge Role

#### 3.4.1 Create IncidentResponseStepFunctionsEventRole

  - Role name: `IncidentResponseStepFunctionsEventRole`
  - Description: `Allows EventBridge to trigger Step Functions`
  - Trusted entity: EventBridge
  - Inline policy name: `StartStepFunctionsPolicy`
  - Inline policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "states:StartExecution",
      "Resource": "arn:aws:states:REGION:ACCOUNT_ID:stateMachine:IncidentResponseStepFunctions"
    }
  ]
}
```

### 3.5 Create VPC Flow Logs Role

#### 3.5.1 Create FlowLogsIAMRole

1.  **Create role**:

      - **Trusted entity**: EC2 (will edit trust policy)
      - **Role name**: `FlowLogsIAMRole`

2.  **Edit trust relationship** to:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "vpc-flow-logs.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

3.  **Add inline policy**:
      - Policy name: `FlowLogsPolicy`
      - Policy JSON:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams",
        "logs:PutLogEvents"
      ],
      "Resource": "*"
    }
  ]
}
```

### 3.6 Create Glue Role

#### 3.6.1 Create GlueCloudWatchRole

  - Role name: `GlueCloudWatchRole`
  - Description: `Allows Glue to access S3 and CloudWatch Logs`
  - Trusted entity: Glue
  - **Managed policies** (attach 3):
      - AWSGlueServiceRole
      - CloudWatchLogsReadOnlyAccess
      - AmazonS3FullAccess
  - **No inline policies needed**

### 3.7 Create IAM Quarantine Policy

#### 3.7.1 Create IrQuarantineIAMPolicy

1.  **Navigate to IAM Console** → **Policies** → **Create policy**

2.  **Policy JSON**:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

3.  **Policy name**: `IrQuarantineIAMPolicy`
4.  **Description**: `Deny-all policy for quarantining compromised IAM users`

-----
