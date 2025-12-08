---
title : "Monitoring Setup"
date: "2000-01-01"
weight : 04
chapter : false
pre : " <b> 5.4. </b> "
---

This Monitoring Setup phase activates and configures the three core log sources for threat detection. It involves enabling CloudTrail for comprehensive management and data events, activating GuardDuty to export security findings to the primary S3 bucket, and setting up VPC Flow Logs on your network to send all traffic metadata to the dedicated CloudWatch Log Group. This ensures a constant, centralized stream of log data is available for processing and automated response.


## Create CloudWatch Log Group

1.  **Open CloudWatch Console** → **Log groups** → **Create log group**

2.  **Configure**:

      - **Log group name**: `/aws/incident-response/centralized-logs`
      - **Retention**: 90 days
      - **KMS key**: None

3.  **Click "Create"**

-----

## Enable AWS CloudTrail

1.  **Open CloudTrail Console** → **Create trail**

2.  **Trail attributes**:

      - **Trail name**: `incident-responses-cloudtrail-ACCOUNT_ID-REGION`
      - **Storage location**: Use existing S3 bucket
      - **S3 bucket**: `incident-response-log-list-bucket-ACCOUNT_ID-REGION`
      - **Log file validation**: Enabled

3.  **Choose log events**:

      - **Management events**: All (Read + Write)
      - **Data events**: S3 - Log all events

4.  **Advanced event selectors** (replace `ACCOUNT_ID` and `REGION`):

<!-- end list -->

```json
[
  {
    "Name": "Log all management events",
    "FieldSelectors": [
      {
        "Field": "eventCategory",
        "Equals": ["Management"]
      }
    ]
  },
  {
    "Name": "Log S3 data events except IR buckets",
    "FieldSelectors": [
      {
        "Field": "eventCategory",
        "Equals": ["Data"]
      },
      {
        "Field": "resources.type",
        "Equals": ["AWS::S3::Object"]
      },
      {
        "Field": "resources.ARN",
        "NotStartsWith": [
          "arn:aws:s3:::incident-response-log-list-bucket-ACCOUNT_ID-REGION/",
          "arn:aws:s3:::processed-guardduty-findings-ACCOUNT_ID-REGION/",
          "arn:aws:s3:::processed-cloudtrail-logs-ACCOUNT_ID-REGION/",
          "arn:aws:s3:::athena-query-results-ACCOUNT_ID-REGION/",
          "arn:aws:s3:::processed-cloudwatch-logs-ACCOUNT_ID-REGION/"
        ]
      }
    ]
  }
]
```

5.  **Create trail**

-----

## Enable Amazon GuardDuty

1.  **Open GuardDuty Console** → **Get Started** → **Enable GuardDuty**

2.  **Configure settings**:

      - **Finding export frequency**: Update CWE and S3 every 15 minutes
      - **S3 export**: `incident-response-log-list-bucket-ACCOUNT_ID-REGION`
      - **KMS encryption**: Choose or create KMS key

-----

## Enable VPC Flow Logs

1.  **Open VPC Console** → **Your VPCs** → Select your VPC

2.  **Actions** → **Create flow log**

3.  **Configure**:

      - **Filter**: All
      - **Aggregation interval**: 10 minutes
      - **Destination**: CloudWatch Logs
      - **Log group**: `/aws/incident-response/centralized-logs`
      - **IAM role**: `FlowLogsIAMRole`
      - **Log format**: Default
  
4.  **Create flow log**

-----

## Enable VPC DNS Query Logging

### Configure Resolver Query Logging

1.  **Open the Amazon Route 53 Console**.
   
2.  In the left navigation pane, select **Resolver** -> **Query logging**.

3.  Click **"Configure query logging"**.

4.  **Configure**:

    * **Name**: Enter a descriptive name, e.g., `IR-DNS-Query-Log-Config`.
    * **Log group**: Select **"Existing log group"** and choose:
        * **`/aws/incident-response/centralized-logs`**

5.  Click **"Next"**.

6.  **VPCs to associate with**:
    * Select the **AWS Region** where your VPC resides.
    * In the **VPCs** section, locate and check the box next to your target VPC ID (the one noted as `YOUR_VPC_ID` in the prerequisites).

7.  Click **"Next"** to review the configuration.

8.  Click **"Submit"**.

-----


