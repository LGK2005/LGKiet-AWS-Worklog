---
title: "Blog 1"
date: "2000-01-01"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# **Triển khai kiến trúc hướng sự kiện với Amazon DynamoDB**

bởi Aman Dhingra, Jack Le Bon và Lee Hannigan | ngày 25 tháng 9 năm 2025 | trong [Advanced (300)](https://aws.amazon.com/blogs/database/category/learning-levels/advanced-300/), [Amazon DynamoDB](https://aws.amazon.com/blogs/database/category/database/amazon-dynamodb/), [Amazon EventBridge](https://aws.amazon.com/blogs/database/category/application-integration/amazon-eventbridge/), [AWS Lambda](https://aws.amazon.com/blogs/database/category/compute/aws-lambda/), [Serverless](https://aws.amazon.com/blogs/database/category/serverless/), [Technical How-to](https://aws.amazon.com/blogs/database/category/post-types/technical-how-to/)

Event-driven architectures﻿ là lựa chọn phổ biến của khách hàng để tích hợp các dịch vụ AWS khác nhau và các hệ thống không đồng nhất. Những kiến trúc này có thể giúp giảm chi phí, mở rộng và thất bại các thành phần một cách độc lập, đồng thời hỗ trợ xử lý song song. Đối với các ứng dụng sử dụng DynamoDB, một số có thể cần các chức năng [Time-to-Live](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/TTL.html)﻿ (TTL) nâng cao hơn, trong khi những ứng dụng khác yêu cầu khả năng kích hoạt các hành động downstream﻿, chẳng hạn như gửi email nhắc nhở cho các sự kiện được quản lý bởi ứng dụng. Dù bạn cần loại bỏ dữ liệu ngay lập tức hay kiểm soát chính xác việc lên lịch các sự kiện tương lai, kiến trúc hướng sự kiện có thể giúp bạn đạt được những mục tiêu này một cách hiệu quả.

Trong loạt bài ba phần này, chúng tôi khám phá các cách tiếp cận để triển khai các patterns﻿ sự kiện nâng cao cho các ứng dụng được hỗ trợ bởi DynamoDB. Dưới đây là cái nhìn sơ lược về các chủ đề chúng tôi sẽ đề cập:

* **Phần 1: Tận dụng Amazon EventBridge Scheduler để loại bỏ dữ liệu chính xác** – Khám phá cách sử dụng EventBridge Scheduler để quản lý và loại bỏ dữ liệu từ DynamoDB một cách hiệu quả với độ chính xác gần thời gian thực.

* [**Phần 2**](https://aws.amazon.com/blogs/database/implement-event-driven-architectures-with-amazon-dynamodb-part-2/)**: Sử dụng Global Secondary Index chuyên dụng để quản lý dữ liệu nghiêm ngặt** – Tìm hiểu về việc tạo Global Secondary Index (GSI) chuyên biệt để kiểm soát chính xác việc loại bỏ và quản lý dữ liệu.

* [**Phần 3**](https://aws.amazon.com/blogs/database/implement-event-driven-architectures-with-amazon-dynamodb-part-3/)**: Triển khai Amazon EventBridge Scheduler để lên lịch sự kiện chi tiết** – Khám phá cách EventBridge Scheduler có thể cho phép lên lịch chi tiết các sự kiện downstream﻿, cho phép quản lý sự kiện tương lai chính xác.

Trong bài viết này (Phần 1), chúng tôi tập trung vào việc cải thiện chức năng TTL bản địa của DynamoDB bằng cách triển khai loại bỏ dữ liệu gần thời gian thực sử dụng EventBridge Scheduler, giảm thời gian điển hình để xóa các items﻿ hết hạn từ vài ngày xuống dưới một phút.

## **DynamoDB Time-to-Live: Chức năng bản địa và Hạn chế**

Time to Live﻿ (TTL), một chức năng bản địa trong DynamoDB, cho phép bạn định nghĩa thời gian hết hạn cụ thể cho các items﻿ trong bảng. Khi TTL được kích hoạt và một attribute﻿ hết hạn được đặt cho một item﻿, DynamoDB tự động loại bỏ item﻿ đó khỏi bảng khi thời gian hết hạn được đạt tới. Tính năng này thường được sử dụng để quản lý dữ liệu có thời gian sống hạn chế, chẳng hạn như records﻿ session﻿ tạm thời, thông tin được cache﻿, hoặc dữ liệu nhạy cảm thời gian khác. Với TTL, bạn có thể tự động hóa quá trình dọn dẹp dữ liệu, tối ưu hóa quản lý dữ liệu, chi phí và hiệu quả lưu trữ.

Để sử dụng TTL bản địa, bạn phải kích hoạt nó trên bảng và chỉ định một attribute﻿ sẽ chứa thời gian hết hạn cho mỗi item﻿. Attribute﻿ này phải ở [định dạng thời gian Unix epoch﻿](https://en.wikipedia.org/wiki/Unix_time). TTL bản địa không tiêu thụ write throughput﻿ trên bảng của bạn và không phát sinh chi phí bổ sung trừ trường hợp global tables﻿, nơi việc xóa TTL trên vùng nguồn không phát sinh chi phí, nhưng việc xóa được replicate﻿ sang các bảng replica﻿ khác tiêu thụ write throughput﻿.

Tuy nhiên, mặc dù tính năng TTL bản địa hiệu quả trong việc tự động hết hạn các items﻿, độ trễ vốn có lên đến vài ngày trước khi xóa có thể không phù hợp với yêu cầu quản lý dữ liệu hoặc tuân thủ của mọi ứng dụng. Sự khác biệt này trở nên đặc biệt liên quan đối với các hệ thống yêu cầu loại bỏ dữ liệu nhanh chóng và chính xác, chẳng hạn như các nền tảng thương mại điện tử cần cập nhật hàng tồn kho nhanh chóng hoặc các session﻿ người dùng động yêu cầu theo dõi thời gian thực chính xác. Item﻿ sẽ tiếp tục xuất hiện trong kết quả query﻿ cho đến khi việc xóa được thực hiện. Một entry﻿ sẽ xuất hiện trong bất kỳ [change stream](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html)﻿ nào được cấu hình khi việc xóa xảy ra.

## **Tổng quan Giải pháp: TTL Gần Thời gian Thực với EventBridge Scheduler**

Để giải quyết những trường hợp sử dụng nhạy cảm thời gian này, chúng ta có thể tích hợp EventBridge Scheduler để thực thi việc loại bỏ items﻿ kịp thời trong vòng 1-2 phút, cung cấp một giải pháp ngay lập tức và có thể dự đoán hơn cho các ứng dụng yêu cầu hết hạn dữ liệu nhanh chóng.

## **Lợi ích của TTL Gần Thời gian Thực**

* **Độ chính xác gần thời gian thực**: Đảm bảo dữ liệu không còn cần thiết được loại bỏ kịp thời, điều này quan trọng đối với các ứng dụng như thương mại điện tử nơi mức hàng tồn kho chính xác là quan trọng để ngăn chặn bán quá mức hoặc bán thiếu sản phẩm.

* **Trải nghiệm người dùng nâng cao**: Loại bỏ ngay lập tức dữ liệu session﻿ hết hạn đảm bảo người dùng tương tác với thông tin hiện tại nhất, cung cấp trải nghiệm tốt hơn và ngay lập tức.

* **Thông báo kịp thời**: Hỗ trợ thực thi bất kỳ quy trình phụ thuộc nào dựa vào việc hết hạn dữ liệu, chẳng hạn như timeout﻿ session﻿ hoặc quyền truy cập tạm thời, được thực thi chính xác khi cần thiết.

* **Tối ưu hóa sử dụng tài nguyên**: Kịp thời giải phóng không gian lưu trữ bằng cách loại bỏ dữ liệu hết hạn, giảm chi phí liên quan đến lưu trữ thông tin lỗi thời.

* **Quản lý dữ liệu được cải thiện**: Chỉ dữ liệu liên quan và hiện tại được giữ lại, đơn giản hóa quản lý dữ liệu và làm cho việc duy trì tính toàn vẹn dữ liệu dễ dàng hơn.

## **Kiến trúc Giải pháp**

Sơ đồ sau minh họa kiến trúc giải pháp của chúng tôi:

![][image1]

Giải pháp chứa các thành phần chính sau:

* [**Amazon DynamoDB**](https://aws.amazon.com/dynamodb/) – Cơ sở dữ liệu NoSQL phân tán, serverless﻿, được quản lý hoàn toàn này được thiết kế để chạy các ứng dụng hiệu suất cao ở bất kỳ quy mô nào. Bảng phải chứa một attribute﻿ cho biết thời gian hết hạn item﻿.

* [**DynamoDB Streams**](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) – Chức năng bản địa này ghi lại một chuỗi theo thứ tự thời gian các sửa đổi cấp item﻿ trong Bảng DynamoDB của bạn. Những sửa đổi này bao gồm các hoạt động Insert﻿, Update﻿ và Delete﻿.

* [**AWS Lambda**](https://aws.amazon.com/pm/lambda/) – Dịch vụ tính toán serverless﻿ này cho phép bạn chạy mã mà không cần cung cấp hoặc quản lý servers﻿.

* [**Amazon EventBridge Scheduler**](https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html) – Scheduler﻿ serverless﻿ này cho phép bạn tạo, chạy và quản lý các tasks﻿ từ một dịch vụ được quản lý tập trung.

## **Cách thức Hoạt động**

Đối với mỗi item﻿ DynamoDB có giá trị TTL, chúng ta liên kết một [one-time invocation](https://docs.aws.amazon.com/scheduler/latest/UserGuide/schedule-types.html#one-time)﻿ schedule﻿ trong EventBridge Scheduler. One-time invocation﻿ được lên lịch để kích hoạt cùng lúc với giá trị TTL của DynamoDB Item.

Mỗi schedule﻿ phải bao gồm một [target﻿](https://docs.aws.amazon.com/scheduler/latest/UserGuide/managing-targets.html) để được sử dụng khi schedule﻿ được gọi. Chúng ta sẽ sử dụng [Universal Target﻿](https://docs.aws.amazon.com/scheduler/latest/UserGuide/managing-targets-universal.html) để gọi trực tiếp DynamoDB [DeleteItem API﻿](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DeleteItem.html), loại bỏ item﻿ khỏi bảng. Tích hợp trực tiếp với DynamoDB hiệu quả về chi phí hơn so với việc gọi Lambda để thực hiện việc xóa.

Luồng hoạt động như sau:

1. Khi một record﻿ được thêm, sửa đổi hoặc loại bỏ khỏi bảng DynamoDB, một stream record﻿ được tạo trong DynamoDB Stream liên quan.

2. DynamoDB Stream gọi một function﻿ Lambda xử lý stream event﻿.

3. Function﻿ Lambda trích xuất primary key﻿ và giá trị TTL của item﻿, sau đó tạo, cập nhật hoặc xóa schedule﻿ EventBridge tương ứng.

4. EventBridge Schedule được cấu hình để gọi DynamoDB DeleteItem API tại thời gian TTL được chỉ định, loại bỏ item﻿ khỏi bảng.

5. Schedule﻿ tự động xóa chính nó sau khi hoàn thành thành công.

EventBridge Schedule liên quan đến một item﻿ được đặt để kích hoạt tại thời gian TTL item﻿ được chỉ định. Các schedule﻿ có ngày trong tương lai thường sẽ kích hoạt trong vòng một phút từ thời gian schedule﻿ khi không sử dụng tính năng [flexible time window](https://docs.aws.amazon.com/scheduler/latest/UserGuide/managing-schedule-flexible-time-windows.html)﻿.

## **Điều kiện Tiên quyết**

Trước khi triển khai giải pháp này, bạn nên có:

* **Tài khoản AWS** – Truy cập vào [tài khoản AWS](https://aws.amazon.com/free/) đang hoạt động để kiểm tra giải pháp

* **Kiến thức cơ bản DynamoDB** – Hiểu biết nền tảng về [các khái niệm DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html)

* **Functions﻿ Lambda** – Quen thuộc với [Lambda để xử lý các events﻿ DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.Lambda.html)

* **Kiến thức cơ bản EventBridge** – Kiến thức cơ bản về EventBridge để [thiết lập scheduler rules﻿](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule-schedule.html)

* **Thành thạo [AWS CLI](https://aws.amazon.com/cli/) hoặc console﻿** – Để cấu hình dịch vụ và giám sát logs﻿. Chúng tôi sử dụng [AWS Management Console](https://aws.amazon.com/console/) trong suốt bài viết này.

## **Các Bước Triển khai**

Đầu tiên, bạn tạo một bảng DynamoDB với DynamoDB Streams được kích hoạt:

1. Trên [DynamoDB console](https://console.aws.amazon.com/dynamodbv2/home), chọn **Tables** trong navigation pane﻿.

2. Chọn **Create table**.

3. Đối với **Table name**, nhập tên cho bảng mới của bạn (chẳng hạn: TTL-table).

4. Đối với **Partition key**, nhập PK làm tên và chọn **String** làm loại.

5. Đối với **Sort key**, nhập SK làm tên và chọn **String** làm loại.

![][image2]

6. Để tất cả các cấu hình khác ở mặc định và chọn **Create table**.

7. Chọn **Tables** trong navigation pane﻿ và mở chi tiết bảng của bạn.

8. Trên tab **Exports and streams**, dưới **DynamoDB stream details**, chọn **Turn on**.

![][image3]

9. Trong wizard﻿ **Turn on DynamoDB stream**, chọn **New and old images**, sau đó chọn **Turn on stream**.

![][image4]

10. Bây giờ bạn sẽ thấy chi tiết DynamoDB stream, với "Stream Status" được đặt thành "On". Hãy chắc chắn ghi chú "Latest stream ARN", bạn sẽ cần nó sau này.

Điều này sẽ kích hoạt DynamoDB Streams trên bảng để hiển thị cả trạng thái cũ và mới của các items﻿ trong stream records﻿, vì vậy bạn có thể quản lý các cập nhật về giá trị TTL của items﻿. Tiếp theo, chúng ta tạo function﻿ Lambda được gọi bởi DynamoDB Stream.

## **Cấu hình Quyền IAM**

EventBridge Scheduler cần quyền thích hợp để gọi DynamoDB DeleteItem API. [Tạo một role﻿ IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html) với trust policy﻿ sau:

```
{
  "Version": "2012-10-17",
  "Statement":
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "scheduler.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
}
```

Và đính kèm một policy﻿ với các quyền sau:

```
{
  "Version": "2012-10-17",
  "Statement":
    {
      "Effect": "Allow",
      "Action": "dynamodb:DeleteItem",
      "Resource": "arn:aws:dynamodb:\[REGION\_NAME\]:\[ACCOUNT\_ID\]:table/\[TABLE\_NAME\]"
    }
}
```

Ghi chú ARN cho role﻿ này, vì chúng ta sẽ cần nó sau này.

**Tạo Lambda function**

Lambda function có trách nhiệm cấu hình EventBridge Scheduler để thực hiện việc xóa chọn lọc các item cụ thể từ DynamoDB table:

1. Trên [Lambda console](https://console.aws.amazon.com/lambda/home), chọn mục **Functions** trong menu điều hướng.

2. Chọn **Create function**.

3. Chọn **Author from scratch**.

4. Với trường **Function name**, nhập tên (ví dụ: DDBStreamTriggerEventScheduler).

5. Chọn một Runtime. Bài viết sử dụng Python 3.11, nhưng bạn có thể chọn bất kỳ runtime nào quen thuộc.

6. Thêm vào Lambda execution role hai policy IAM là AWSLambdaDynamoDBExecutionRole và một inline policy có quyền scheduler:CreateSchedule.

7. Chọn **Create function**.

![][image5]

8. Bấm vào tab **Configuration** của Lambda Function và chọn bảng **Permissions**.

9. Trong phần **Execution role**, nhấn vào **Role name** liên kết. Tên này thường có dạng như YourLambdaFunctionName-Role-abc.

10. Chọn **Add permissions**, sau đó chọn **Create inline policy**

11. Chuyển từ giao diện Visual sang JSON và thêm chính sách sau, cấp quyền cho Lambda function truy cập DynamoDB Stream (nhớ nhập ARN DynamoDB Stream đã note trước đó) và tạo, cập nhật, xóa EventBridge Schedules:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DynamoDBStreamAccess",
      "Effect": "Allow",
      "Action": [
        "dynamodb:GetShardIterator",
        "dynamodb:DescribeStream",
        "dynamodb:GetRecords"
      ],
      "Resource": "[Your DynamoDB Stream ARN]"
    },
    {
      "Sid": "DynamoDBListStreams",
      "Effect": "Allow",
      "Action": "dynamodb:ListStreams",
      "Resource": "*"
    },
    {
      "Sid": "EventBridgeSchedulerAccess",
      "Effect": "Allow",
      "Action": [
        "scheduler:CreateSchedule",
        "scheduler:UpdateSchedule",
        "scheduler:DeleteSchedule"
      ],
      "Resource": "arn:aws:scheduler:*:*:schedule/default/dynamodb_ttl_*"
    },
    {
      "Sid": "PassRoleToScheduler",
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "iam:PassedToService": "scheduler.amazonaws.com"
        }
      }
    }
  ]
}
```

![][image6]

1.  Chọn **Next**.

2.  Nhập **Policy name**, ví dụ: AWSLambdaToEventBridgeScheduler.

3.  Chọn **Create policy**.

![][image7]

15. Quay lại [DynamoDB console](https://console.aws.amazon.com/dynamodbv2/home), chọn **Tables** ở menu và chọn bảng vừa tạo (TTL-Table).

16. Chuyển sang tab **Exports and streams**.

17. Ở vùng **Trigger**, nhấn **Create trigger**.

18. Tại phần **AWS Lambda function details**, chọn Lambda function vừa tạo (DDBStreamTriggerEventScheduler).

![][image8]

19. Chọn **Create trigger**.

20. Quay lại [Lambda console](https://console.aws.amazon.com/lambda/home), chọn **Functions** ở menu, tìm và chọn Lambda function vừa tạo.

21. Tại tab **Code** của Lambda function, thêm mã code phù hợp.  
    **Code Mẫu Lambda Function (Python)**

```
import json
import datetime
import boto3
import os
import logging

logger = logging.getLogger()
logger.setLevel(logging.INFO)

ROLE_ARN = os.environ.get('SCHEDULER_ROLE_ARN')
TABLE_NAME = os.environ.get('DYNAMODB_TABLE_NAME')
TIMEZONE = os.environ.get('TIMEZONE', 'UTC')

scheduler_client = boto3.client('scheduler')

def lambda_handler(event, context):
    try:
        if not event.get('Records'):
            logger.error("No records found in the event")
            return {
                'statusCode': 400,
                'body': json.dumps('No records found in the event')
            }
        
        for record in event['Records']:
            process_record(record)
            
        return {
            'statusCode': 200,
            'body': json.dumps('DynamoDB near-real time TTL operations completed successfully.')
        }
    except Exception as e:
        logger.error(f"Error processing event: {str(e)}")
        return {
            'statusCode': 500,
            'body': json.dumps(f'Error: {str(e)}')
        }

def process_record(record):
    try:
        event_name = record.get('eventName')
        
        if event_name == "INSERT":
            handle_insert(record)
        elif event_name == "MODIFY":
            handle_modify(record)
        elif event_name == "REMOVE":
            handle_remove(record)
        else:
            logger.warning(f"Unhandled event type: {event_name}")
    except Exception as e:
        logger.warning(f"Error processing record: {str(e)}")
        raise

def handle_insert(record):
    keys = record['dynamodb']['Keys']
    pk = keys['PK']['S']
    sk = keys['SK']['S']
    ttl_value = record['dynamodb']['NewImage']['ttl']['N']
    
    epoch_dt = datetime.datetime.fromtimestamp(int(ttl_value))
    logger.info(f"Creating schedule for item {pk}#{sk} with TTL at {epoch_dt}")
    
    scheduler_client.create_schedule(
        ActionAfterCompletion="DELETE",
        FlexibleTimeWindow={"Mode": "OFF"},
        Name=f"dynamodb_ttl_{pk}_{sk}",
        ScheduleExpression=f"at({epoch_dt.strftime('%Y-%m-%dT%H:%M:%S')})",
        ScheduleExpressionTimezone=TIMEZONE,
        Target={
            "RoleArn": ROLE_ARN,
            "Arn": "arn:aws:scheduler:::aws-sdk:dynamodb:deleteItem",
            "Input": json.dumps({"Key": {"PK": {"S": pk}, "SK": {"S": sk}}, "TableName": TABLE_NAME})
        }
    )

def handle_modify(record):
    if 'OldImage' not in record['dynamodb'] or 'NewImage' not in record['dynamodb']:
        return
        
    if 'ttl' not in record['dynamodb']['OldImage'] or 'ttl' not in record['dynamodb']['NewImage']:
        return
        
    keys = record['dynamodb']['Keys']
    pk = keys['PK']['S']
    sk = keys['SK']['S']
    old_ttl = record['dynamodb']['OldImage']['ttl']['N']
    new_ttl = record['dynamodb']['NewImage']['ttl']['N']
    
    if old_ttl != new_ttl:
        epoch_dt = datetime.datetime.fromtimestamp(int(new_ttl))
        logger.info(f"Updating schedule for item {pk}#{sk} with new TTL at {epoch_dt}")
        
        try:
            scheduler_client.update_schedule(
                Name=f"dynamodb_ttl_{pk}_{sk}",
                FlexibleTimeWindow={"Mode": "OFF"},
                ScheduleExpression=f"at({epoch_dt.strftime('%Y-%m-%dT%H:%M:%S')})",
                ScheduleExpressionTimezone=TIMEZONE,
                Target={
                    "RoleArn": ROLE_ARN,
                    "Arn": "arn:aws:scheduler:::aws-sdk:dynamodb:deleteItem",
                    "Input": json.dumps({"Key": {"PK": {"S": pk}, "SK": {"S": sk}}, "TableName": TABLE_NAME})
                }
            )
        except scheduler_client.exceptions.ResourceNotFoundException:
            handle_insert(record)

def handle_remove(record):
    keys = record['dynamodb']['Keys']
    pk = keys['PK']['S']
    sk = keys['SK']['S']
    logger.info(f"Deleting schedule for item {pk}#{sk}")
    
    try:
        scheduler_client.delete_schedule(Name=f'dynamodb_ttl_{pk}_{sk}')
    except scheduler_client.exceptions.ResourceNotFoundException:
        pass
```

22. Chọn **Deploy** để cập nhật mã function mới nhất.

23. Cuối cùng, thêm tên DynamoDB table và ARN của EventBridge Scheduler role vào biến môi trường của Lambda Function. Vào tab **Configuration** và phần **Environment variables**.

24. Chọn **Edit**.

25. Chọn **Add environment variable** rồi nhập:

    1. **Key:** DYNAMODB\_TABLE\_NAME

    2. **Value:** Tên bảng của bạn (ví dụ TTL-Table)

26. Chọn **Add environment variable** một lần nữa, rồi nhập:

    1. **Key:** SCHEDULER\_ROLE\_ARN

    2. **Value:** ARN của scheduler role đã ghi chú trước đó (dạng arn:aws:iam::\*\*\*\*:role/eventbridge\_scheduler\_role)

![][image9]

27. Chọn **Save**

## **Kiểm tra Giải pháp**

Bạn có thể kiểm tra giải pháp bằng cách thêm các items﻿ vào bảng DynamoDB của bạn với giá trị TTL. Dưới đây là ví dụ tạo 10 items﻿ mẫu với giá trị TTL sử dụng AWS CLI:

```
#!/bin/bash
TABLE="TTL-Table"
for PK_VALUE in {1..10}; do
    ISO_TIMESTAMP_PLUS_3_MINS=$(date -v+3M -u +"%Y-%m-%dT%H:%M:%S")
    
    aws dynamodb put-item --table-name $TABLE \
        --item '{"PK": {"S": "'$PK_VALUE'"}, "SK": {"S": "StaticSK"}, "REMINDER_TIMESTAMP": {"S": "'$ISO_TIMESTAMP_PLUS_3_MINS'"}, "email": {"S": "abc@example.com"}, "ATTR_1": {"S": "This is a static attribute"}}'
done
```

## Bạn có thể điều hướng đến tab **Monitoring** của nhóm lịch trình EventBridge để xem các lệnh xóa đang được thực thi. Schedule sẽ kích hoạt các thao tác vào một thời điểm cụ thể; bạn có thể quan sát các lần kích hoạt này bằng cách xem chỉ số InvocationAttemptCount. Trong trường hợp của chúng tôi, các lần kích hoạt là các lệnh xóa được thực hiện đối với bảng DynamoDB. Để xem danh sách tất cả các chỉ số có sẵn cho một nhóm lịch trình, hãy tham khảo [Monitoring Amazon EventBridge Scheduler with Amazon CloudWatch﻿](https://docs.aws.amazon.com/scheduler/latest/UserGuide/monitoring-cloudwatch.html).

![][image10]

## **Cân nhắc Chi phí**

Chi phí của việc sử dụng phương pháp này cho 1,000,000 mục TTL được ước tính trong bảng sau đây cùng với so sánh với việc sử dụng chức năng DynamoDB gốc. Mỗi mục DynamoDB có kích thước dưới 1KB và được lưu trữ trong bảng sử dụng chế độ on-demand tại vùng us-east-1. Các tầng miễn phí không được xem xét trong phân tích này.

|  | TTL gần thời gian thực | TTL DynamoDB bản địa |
| :---- | :---- | :---- |
| DynamoDB Stream | $0 (DynamoDB streams miễn phí khi được tiêu thụ bởi Lambda) | \- |
| Lambda |  |  |
| EventBridge Scheduler | $1 | \- |
| DynamoDB Delete | $0.63 | \- |
| **Tổng Chi phí** | $1.63 | $0 |

## **Hạn chế**

Giới hạn trên cho giải pháp này liên quan đến [hạn mức tốc độ yêu cầu của EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/scheduler-quotas.html). Các yêu cầu CreateSchedule, UpdateSchedule và DeleteSchedule mặc định đều có giới hạn tối đa là 1.000 yêu cầu mỗi giây đối với hầu hết các khu vực. Các giới hạn này có thể điều chỉnh được. Nếu giới hạn này bị vượt quá, EventBridge Scheduler sẽ từ chối mọi yêu cầu cho thao tác đó trong khoảng thời gian còn lại. [Dead-Letter Queue \- DLQ có thể được sử dụng để bắt và thực hiện lại các lần thực thi thất bại](https://docs.aws.amazon.com/scheduler/latest/UserGuide/configuring-schedule-dlq.html).

EventBridge Scheduler cũng áp dụng một giới hạn điều tiết (throttle limit) đối với lượt gọi mục tiêu (target invocations) đồng thời, mặc định là 1.000 lượt gọi mỗi giây ở hầu hết các khu vực. Điều này đề cập đến tốc độ mà các schedule payloads được gửi đến các mục tiêu của chúng. Nếu giới hạn này bị vượt quá, các lượt gọi sẽ không bị hủy bỏ mà bị điều tiết (throttled), nghĩa là chúng sẽ bị trì hoãn và được xử lý sau. Hạn mức (quota) này có thể điều chỉnh và có thể mở rộng lên đến hàng chục nghìn lượt gọi mỗi giây.

Cuối cùng, một giới hạn trên khác đối với giải pháp này sẽ là [hạn mức thực thi đồng thời (concurrent executions quota)](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html) có sẵn cho các Lambda function của bạn, mặc định là 1.000 mỗi giây cho mỗi tài khoản. Đây là một giới hạn quan trọng cần xem xét, đặc biệt nếu bạn có các Lambda function khác đang chạy trong cùng một tài khoản. Nếu bạn đạt đến giới hạn đồng thời (concurrency limit), function của bạn sẽ bị điều tiết (throttled). Giới hạn này [có thể được tăng lên](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html).

## **Dọn dẹp**

Nếu bạn tạo môi trường kiểm tra để theo dõi bài viết này, hãy chắc chắn:

1. [Xóa DynamoDB table](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/getting-started-step-6.html)

2. [Xóa﻿ Lambda function](https://docs.aws.amazon.com/lambda/latest/dg/urls-webhook-tutorial.html#urls-webhook-tutorial-cleanup)

3. [Xóa EventBridge schedule](https://docs.aws.amazon.com/scheduler/latest/UserGuide/managing-schedule-delete.html)

4. [Xóa bất kỳ roles﻿ IAM nào được tạo trong quá trình này](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_delete.html)

5. Xóa bất kỳ tài nguyên nào khác bạn đã tạo để kiểm tra giải pháp

## **Kết luận**

Trong bài viết này, chúng tôi đã khám phá cách bạn có thể sử dụng Amazon EventBridge Scheduler để triển khai TTL gần thời gian thực cho DynamoDB, giảm thời gian xóa một item﻿ sau khi TTL hết hạn từ vài ngày xuống thường dưới 1-2 phút.

Giải pháp serverless﻿ này thu hẹp khoảng cách giữa khả năng tích hợp sẵn của DynamoDB và nhu cầu loại bỏ items﻿ ngay lập tức, có kiểm soát. Mặc dù nó phát sinh một số chi phí bổ sung so với chức năng TTL bản địa, nó giải quyết hiệu quả nhu cầu loại bỏ dữ liệu nhanh chóng và đáng tin cậy.

Cách tiếp cận này có thể mở rộng đến hàng tỷ records﻿ xóa TTL hoạt động, với thời gian xóa tăng lên khi việc xóa đồng thời mở rộng vượt quá giới hạn gọi EventBridge Scheduler.

Trong [Phần 2](https://aws.amazon.com/blogs/database/implement-event-driven-architectures-with-amazon-dynamodb-part-2/), chúng tôi sẽ đi sâu hơn vào việc triển khai loại bỏ dữ liệu nghiêm ngặt trong DynamoDB sử dụng Global Secondary Index. Loạt bài kết thúc với [Phần 3](https://aws.amazon.com/blogs/database/implement-event-driven-architectures-with-amazon-dynamodb-part-3/), nơi chúng tôi sẽ sử dụng Amazon EventBridge Scheduler để lên lịch sự kiện chi tiết. Những patterns﻿ hướng sự kiện này sẽ giúp bạn tự động hóa các quy trình chính, duy trì dữ liệu chính xác và đáp ứng các yêu cầu ứng dụng chính xác với nỗ lực thủ công tối thiểu.

---

**Về các tác giả**

| ![][image11] Lee Hannigan [Lee](https://www.linkedin.com/in/lee-hannigan/) Hannigan là Chuyên gia giải pháp DynamoDB cao cấp (Sr. DynamoDB Specialist Solutions Architect) làm việc tại Donegal, Ireland. Anh có chuyên môn sâu rộng về các hệ thống phân tán (distributed systems), cùng nền tảng vững chắc về các công nghệ dữ liệu lớn (big data) và phân tích (analytics technologies). Trong vai trò Chuyên gia giải pháp DynamoDB, Lee xuất sắc trong việc hỗ trợ khách hàng thiết kế, đánh giá và tối ưu hóa khối lượng công việc (workloads) sử dụng các khả năng của DynamoDB. |
| :---- |

| ![][image12] Aman Dhingra [Aman](https://www.linkedin.com/in/amdhing/) Dhingra là Chuyên gia giải pháp DynamoDB cao cấp (Sr. DynamoDB Specialist Solutions Architect) làm việc tại Dublin, Ireland. Anh có đam mê về các hệ thống phân tán (distributed systems) và nền tảng chuyên sâu về dữ liệu lớn & phân tích (big data & analytics). Aman là tác giả của cuốn "Amazon DynamoDB – The Definitive Guide" và hỗ trợ khách hàng trong việc thiết kế, đánh giá và tối ưu hóa khối lượng công việc vận hành trên Amazon DynamoDB. |
| :---- |

| ![][image13] Jack Le Bon [Jack](https://www.linkedin.com/in/jack-le-bon/) Le Bon là Kiến trúc sư giải pháp (Solutions Architect) làm việc tại London, chuyên hỗ trợ các khách hàng thuộc lĩnh vực truyền thông và giải trí (Media & Entertainment). Là thành viên của nhóm chuyên về công nghệ không máy chủ (Serverless speciality group) tại AWS, Jack giúp khách hàng thiết kế và triển khai kiến trúc hướng sự kiện (event-driven architectures) sử dụng các công nghệ Serverless. Jack tập trung vào hỗ trợ các tổ chức xây dựng kiến trúc hiệu quả, giúp họ tập trung vào hoạt động kinh doanh cốt lõi thay vì quản lý hạ tầng. |
| :---- |

[image1]: /images/3-Blog/Blog-1/image_1.png
[image2]: /images/3-Blog/Blog-1/image_2.png
[image3]: /images/3-Blog/Blog-1/image_3.png
[image4]: /images/3-Blog/Blog-1/image_4.png
[image5]: /images/3-Blog/Blog-1/image_5.png
[image6]: /images/3-Blog/Blog-1/image_6.png
[image7]: /images/3-Blog/Blog-1/image_7.png
[image8]: /images/3-Blog/Blog-1/image_8.png
[image9]: /images/3-Blog/Blog-1/image_9.png
[image10]: /images/3-Blog/Blog-1/image_10.png
[image11]: /images/3-Blog/Blog-1/image_11.png
[image12]: /images/3-Blog/Blog-1/image_12.png
[image13]: /images/3-Blog/Blog-1/image_13.png
