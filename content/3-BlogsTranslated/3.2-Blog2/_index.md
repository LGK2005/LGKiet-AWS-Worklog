---
title: "Blog 2"
date:  ""
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---
# **Triển khai kiến trúc hướng sự kiện với Amazon DynamoDB – Phần 2﻿**

bởi Aman Dhingra và Lee Hannigan | vào ngày 25 tháng 9 năm 2025 | trong [Advanced (300)](https://aws.amazon.com/blogs/database/category/learning-levels/advanced-300/), [Amazon DynamoDB](https://aws.amazon.com/blogs/database/category/database/amazon-dynamodb/), [Amazon EventBridge](https://aws.amazon.com/blogs/database/category/application-integration/amazon-eventbridge/), [AWS Lambda](https://aws.amazon.com/blogs/database/category/compute/aws-lambda/), [Serverless](https://aws.amazon.com/blogs/database/category/serverless/), [Technical How-to](https://aws.amazon.com/blogs/database/category/post-types/technical-how-to/)

Trong [bài đăng trước﻿](https://aws.amazon.com/blogs/database/implement-event-driven-architectures-with-amazon-dynamodb/) (Phần 1﻿) của loạt bài này, chúng tôi đã thảo luận về cách bạn có thể sử dụng﻿ [Amazon EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html) để quản lý loại bỏ dữ liệu chính xác trong﻿ [Amazon DynamoDB](https://aws.amazon.com/dynamodb/). Trong bài đăng này﻿ (Phần 2﻿), chúng tôi khám phá một phương pháp khác sử dụng﻿ [global secondary indexes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html) (GSIs) để xử lý các yêu cầu Time to Live (TTL) chi tiết﻿.

Khi xử lý các ứng dụng thông lượng cao, việc kiểm soát chính xác về thời hạn dữ liệu là quan trọng. Sự trзадержка tự nhiên trong tính năng TTL gốc của DynamoDB có thể không luôn đáp ứng nhu cầu của các ứng dụng yêu cầu loại bỏ dữ liệu ngay lập tức. Bằng cách sử dụng GSIs, chúng ta có thể tạo ra một hệ thống phản hồi tốt hơn phù hợp với các yêu cầu thời gian thực của ứng dụng.

## **Tổng quan giải pháp﻿**

Sử dụng GSI cho trường hợp sử dụng này cho phép bạn truy vấn hiệu quả dữ liệu đủ điều kiện để loại bỏ theo các khoảng thời gian phù hợp với nhu cầu cụ thể của bạn. Giải pháp này có thể cung cấp độ chi tiết gần thời gian thực, tương tự như phương pháp EventBridge Scheduler trước đó. Bằng cách thiết lập một [sharded GSI](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-indexes-gsi-sharding.html) với thuộc tính TTL làm sort key, chúng ta có thể định kỳ truy vấn và loại bỏ các mục hết hạn với độ chính xác gần 1 phút﻿.

Sơ đồ sau đây minh họa kiến trúc giải pháp﻿.

1. EventBridge Scheduler kích hoạt một Lambda function theo lịch trình đã định﻿.

2. Lambda function truy vấn GSI để lấy các mục được đánh dấu để xóa﻿.

3. Lambda function xóa các mục đã lấy từ bảng DynamoDB﻿.

![][image1]

## **Nhu cầu về sharding﻿**

Để quản lý hiệu quả thông lượng ghi và đọc cao﻿, [sharding](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-indexes-gsi-sharding.html) GSI là cần thiết. Sharding phân phối tải trên nhiều partition, ngăn chặn bất kỳ partition đơn lẻ nào trở thành nút thắt cổ chai và gây ra throttling. Điều này có thể đạt được bằng cách thêm hậu tố ngẫu nhiên hoặc được tính toán vào các giá trị partition key. Bằng cách ngẫu nhiên hóa partition key, các ghi được phân bổ đều hơn trên không gian partition key, cải thiện tính song song và thông lượng tổng thể.

Tuy nhiên, truy vấn dữ liệu hết hạn yêu cầu thực hiện truy vấn cho từng shard trong phạm vi. Cách tiếp cận này đảm bảo rằng tất cả các partition có thể được kiểm tra để tìm các mục hết hạn, nhưng nó đòi hỏi nhiều truy vấn để bao phủ tất cả các shard.

Số lượng shard được tính toán dựa trên thông lượng ghi đỉnh dự kiến của bảng cơ sở bằng công thức sau﻿:

| number of shards \= (peak write capacity units (WCU) / 1000) \+ buffer |
| :---- |

Bao gồm buffer trong tính toán của bạn được khuyến nghị để đảm bảo bạn có đủ shard để tránh throttling trên bảng DynamoDB. Buffer cũng cung cấp phân phối bổ sung để xử lý các đỉnh bất ngờ trong thông lượng ghi, duy trì hiệu suất và độ tin cậy của ứng dụng.

Nếu về lâu dài WCU đỉnh dự kiến của bạn tăng, bạn có thể trước tiên sửa đổi Lambda function để truy vấn các shard bổ sung, và sau đó cập nhật ứng dụng để tăng số lượng shard khi ghi dữ liệu vào bảng. Lambda có thể truy vấn các shard không tồn tại trong GSI mà không gặp vấn đề.

## **Lựa chọn primary key cho GSI﻿**

Khi chọn key cho GSI của bạn, partition key GSI\_PK sẽ được tạo bằng một shard ngẫu nhiên, với phạm vi được xác định bởi tính toán trước đó để đảm bảo phân phối tải đều. Sort key sẽ là thuộc tính TTL của bạn, có thể được biểu diễn bằng số epoch hoặc định dạng thời gian chuỗi. Sự kết hợp này cho phép truy vấn hiệu quả các mục dựa trên thời gian hết hạn, trong khi partition key được chia sẻ giúp phân phối các hoạt động ghi và đọc trên nhiều partition, tối ưu hóa hiệu suất tổng thể và khả năng mở rộng của bảng DynamoDB.

## **Tối ưu chi phí﻿ Indexing**

Duy trì GSI hiệu quả bao gồm việc sử dụng﻿ [KEYS\_ONLY](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_Projection.html) projection và đảm bảo index﻿ [là sparse](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-indexes-general.html#bp-indexes-general-projections). Bằng cách sử dụng﻿ KEYS\_ONLY, GSI chỉ lưu trữ các thuộc tính primary key và các thuộc tính index key, giảm đáng kể chi phí lưu trữ và thông lượng. Ngoài ra, làm cho GSI sparse có nghĩa nó chỉ bao gồm các mục có thuộc tính TTL. Việc lập chỉ mục có chọn lọc này đảm bảo rằng chỉ các mục liên quan, những mục có thời gian hết hạn, được bao gồm trong GSI, tối ưu hóa thêm hiệu suất và sử dụng tài nguyên. Điều này không chỉ nâng cao hiệu quả truy vấn mà còn giúp quản lý chi phí bằng cách giữ cho index gọn và tập trung vào các mục yêu cầu quản lý TTL.

## **Mô hình dữ liệu mẫu﻿**

Mô hình dữ liệu ví dụ sau được thiết kế để xử lý quản lý trạng thái session trong môi trường thông lượng cao, nơi các session yêu cầu quản lý TTL chi tiết. Bằng cách chia sẻ GSI, chúng tôi phân phối tải ghi trên bốn shard để xử lý hiệu quả thông lượng đỉnh 3.000 WCU, với số lượng shard được chọn để bao gồm dung lượng buffer bổ sung vượt quá đỉnh dự kiến. Mỗi mục session bao gồm thuộc tính TTL để đảm bảo hết hạn kịp thời, và partition key của GSI (GSI\_PK) là định danh shard ngẫu nhiên để cho phép truy vấn và xóa chính xác các session hết hạn, duy trì hiệu suất tối ưu và tính toàn vẹn dữ liệu﻿.

Ví dụ sau cho thấy﻿ SessionTable mà chúng tôi đã tạo bằng﻿ [NoSQL Workbench](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/workbench.html):

![][image2]

Ví dụ sau cho thấy TTL GSI của chúng tôi﻿.

![][image3]

Với mô hình dữ liệu này, bây giờ chúng ta có thể truy vấn từng shard bằng GSI, áp dụng điều kiện trên thuộc tính timestamp TTL để xác định các bản ghi cũ hơn thời gian hiện tại. Điều này cho phép chúng ta tìm và loại bỏ hiệu quả các session hết hạn.

## **Điều kiện tiên quyết﻿**

Trước khi đi sâu vào triển khai giải pháp TTL tùy chỉnh, bạn nên có các điều kiện tiên quyết sau﻿:

* **AWS account** – Truy cập vào một﻿ [AWS account](https://aws.amazon.com/free/) hoạt động﻿

* **DynamoDB basics** – Hiểu biết cơ bản về﻿ [DynamoDB concepts](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html), bao gồm bảng, mục, thuộc tính và các hoạt động CRUD cơ bản, là cần thiết để cấu hình và quản lý cơ sở dữ liệu hiệu quả﻿

* **Sharding** – Hiểu biết cơ bản về﻿ [sharding techniques](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-indexes-gsi-sharding.html) cho DynamoDB﻿

* **Lambda functions** – Bạn nên quen thuộc với﻿ [AWS Lambda](http://aws.amazon.com/lambda), vì bạn sẽ tạo và triển khai các Lambda function để truy vấn DynamoDB và chạy logic TTL tùy chỉnh﻿

* **EventBridge basics** – Kiến thức cơ bản về﻿ [Amazon EventBridge](https://aws.amazon.com/eventbridge/) là cần thiết để thiết lập﻿ [EventBridge Scheduler rules](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule-schedule.html) để gọi Lambda function theo các khoảng thời gian cụ thể﻿

* [**AWS CLI**](https://aws.amazon.com/cli/) **hoặc thành thạo console﻿** – Để cấu hình dịch vụ và giám sát log. Chúng tôi sử dụng﻿ [AWS Management Console](https://aws.amazon.com/console/) trong suốt bài đăng này﻿.

## **Tạo bảng DynamoDB với GSI﻿**

Bước đầu tiên của chúng tôi là tạo bảng DynamoDB với﻿ [Amazon DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) được bật. Hoàn thành các bước sau﻿:

1. Trên console DynamoDB, chọn﻿ **Tables** trong navigation pane﻿.

2. Chọn﻿ **Create table**.

3. Với **Table name**, nhập tên cho bảng mới của bạn﻿.

4. Với **Partition key**, nhập PK làm tên và chọn﻿ **String** làm kiểu﻿.

5. Với **Sort key**, nhập SK làm tên và chọn﻿ **String** làm kiểu﻿.

![][image4]

6. Với **Table settings**, chọn﻿ **Customize settings**. Tùy chọn﻿ **Customize settings** cũng cho phép bạn định nghĩa secondary index trên bảng, bật/tắt deletion protection, chuyển đổi kiểu mã hóa và thêm resource tag khi tạo bảng.

7. Với **Read/write capacity settings**, đảm bảo﻿ **On-demand** được chọn﻿.

![][image5]

8. Cuộn xuống phần﻿ **Secondary indexes**.

9. Chọn﻿ **Create global index**.

10. Với **Partition key**, nhập GSI\_PK làm tên và chọn﻿ **String** làm kiểu﻿.

11.  Cho﻿ **Sort key**, nhập TTL làm tên và chọn﻿ **String** làm kiểu﻿.

12. Với﻿ **Index name**, nhập tên cho index mới của bạn﻿.

13. Với **Attribute projections**, chọn﻿ **Only keys**.

14. Chọn﻿ **Create index**.

![][image6]

15. Để tất cả cấu hình khác mặc định và chọn﻿ **Create table**.

## **Tạo Lambda﻿ function**

Tiếp theo, chúng tôi cấu hình Lambda function sẽ được gọi bởi EventBridge Scheduler. Lambda function này chịu trách nhiệm lấy các mục đủ điều kiện để xóa và xóa những mục đó. Hoàn thành các bước sau:

1. Trên console Lambda, chọn﻿ **Functions** trong navigation pane﻿.

2. Chọn﻿ **Create function**.

3. Chọn﻿ **Author from scratch**.

4. Với **Function name**, nhập tên (ví dụ, StrictDataManagement)﻿.

5. Với **Runtime**, chọn﻿ **Python 3.12**.

6. [Thêm policy](https://docs.aws.amazon.com/lambda/latest/dg/lambda-permissions.html) cho phép hàm đọc từ GSI và ghi vào bảng﻿:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ReadFromGSI",
            "Effect": "Allow",
            "Action": [
                "dynamodb:Query"
            ],
            "Resource": [
                "arn:aws:dynamodb:<region>:<account-id>:table/<table-name>",
                "arn:aws:dynamodb:<region>:<account-id>:table/<table-name>/index/<gsi-name>"
            ]
        },
        {
            "Sid": "WriteToTable",
            "Effect": "Allow",
            "Action": [
                "dynamodb:DeleteItem"
            ],
            "Resource": "arn:aws:dynamodb:<region>:<account-id>:table/<table-name>"
        }
    ]
}
```

7. Chọn﻿ **Create function**.

![][image7]

8. Trên tab﻿ **Code** của Lambda function, thay thế mã Lambda function bằng mã sau. Thay đổi các hằng số﻿, SHARDS, TABLE\_NAME, và﻿ INDEX\_NAME để phù hợp với yêu cầu cụ thể của bạn﻿:

```
import boto3
from datetime import datetime

# Constants
SHARDS = 4
TABLE_NAME = 'TTL-Table'
INDEX_NAME = 'TTL-index'

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table(TABLE_NAME)

def lambda_handler(event, context):
    current_time = datetime.now().replace(second=0, microsecond=0).isoformat()
    
    for shard in range(SHARDS):
        shard_key = str(shard)
        query_and_delete_expired_items(shard_key, current_time)

def query_and_delete_expired_items(shard_key, current_time):
    last_evaluated_key = None
    
    while True:
        # Print for logging purposes
        print(f"Checking shard {shard_key} at {current_time}")
        
        query_kwargs = {
            'IndexName': INDEX_NAME,
            'KeyConditionExpression': 'GSI_PK = :shard AND #ttl < :current_time',
            'ExpressionAttributeValues': {
                ':shard': shard_key,
                ':current_time': current_time
            },
            'ExpressionAttributeNames': {
                '#ttl': 'TTL'
            }
        }
        
        if last_evaluated_key:
            query_kwargs['ExclusiveStartKey'] = last_evaluated_key
            
        response = table.query(**query_kwargs)
        
        items_to_delete = response.get('Items', [])
        
        if items_to_delete:
            delete_expired_items(items_to_delete)
        
        last_evaluated_key = response.get('LastEvaluatedKey')
        
        if not last_evaluated_key:
            break

def delete_expired_items(items):
    with table.batch_writer() as batch:
        for item in items:
            batch.delete_item(
                Key={
                    'PK': item['PK'],
                    'SK': item['SK']
                }
            )
```
9. Chọn﻿ **Deploy** để triển khai mã hàm mới nhất﻿.

Hàm﻿ delete\_expired\_items sử dụng﻿ [Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) [batch\_writer](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/dynamodb/table/batch_writer.html) để thực hiện xóa hàng loạt cho hiệu quả. Tuy nhiên﻿, batch\_writer không hỗ trợ﻿ [ConditionExpression](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.OperatorsAndFunctions.html), có nghĩa là không có cách nào để kiểm tra xem một mục vẫn đủ điều kiện để xóa tại thời điểm ghi. Điều này có thể rủi ro trong các trường hợp sử dụng mà giá trị TTL có thể đã thay đổi giữa lần đọc ban đầu và nỗ lực xóa. Để tránh vô tình xóa các mục không còn hết hạn, được khuyến nghị sử dụng hoạt động DeleteItem với﻿ ConditionExpression xác minh giá trị TTL vẫn trong phạm vi mong đợi﻿.

## **Tạo EventBridge Scheduler﻿**

Bây giờ chúng tôi tạo lịch trình EventBridge gọi Lambda function mỗi 5 phút. Bạn có thể điều chỉnh khoảng thời gian này để phù hợp với yêu cầu loại bỏ dữ liệu của bạn.

1. Trên console EventBridge, chọn﻿ **Schedules** trong navigation pane﻿.

2. Chọn﻿ **Create schedule**.

3. Cho﻿ **Schedule name**, nhập tên cho lịch trình mới của bạn﻿.

4. Cho﻿ **Schedule pattern**, chọn﻿ **Recurring schedule**.

5. Cho﻿ **Schedule type**, chọn﻿ **Rate-based schedule**.

6. Cho﻿ **Rate expression**, đặt﻿ **Value** thành﻿ **5** và﻿ **Unit** thành﻿ **minutes**.

7. Cho﻿ **Flexible time window**, chọn﻿ **Off**.

8. Để tất cả cấu hình khác mặc định và chọn﻿ **Next**.

![][image8]

9. Cho﻿ **Templated targets**, chọn﻿ **AWS Lambda Invoke**.

10. Cho﻿ **Lambda function**, chọn Lambda function StrictDataManagement của bạn﻿.

11. Để tất cả cấu hình khác mặc định và chọn﻿ **Next**.

![][image9]

12. Cho﻿ **Action after schedule completion**, chọn﻿ **NONE**.

13. Để tất cả cấu hình khác mặc định và chọn﻿ **Next**.

![][image10]

14. Xem lại cấu hình của bạn và chọn﻿ **Create schedule**.

![][image11]

## **Tạo mục mẫu để xem giải pháp hoạt động﻿**

Bạn có thể kiểm tra giải pháp bằng cách thêm mục vào bảng DynamoDB với giá trị TTL. Đây là ví dụ tạo 10 mục mẫu với giá trị TTL bằng AWS CLI﻿:

```
#!/bin/bash

TABLE="TTL-Table"

for PK_VALUE in {1..10}; do
    # Get current UTC time in ISO 8601 format
    ISO_TIMESTAMP=$(date -u +"%Y-%m-%dT%H:%M:%SZ")
    
    # Generate a random shard value between 0 and 3
    GSI_PK_VALUE=$((RANDOM % 4))  
    
    # Put item into DynamoDB
    aws dynamodb put-item --table-name $TABLE \
    --item '{"PK": {"S": "'$PK_VALUE'"}, "SK": {"S": "StaticSK"}, "GSI_PK": {"S": "'$GSI_PK_VALUE'"}, "TTL": {"S": "'$ISO_TIMESTAMP'"}, "SessionData": {"S": "{\"cart\": \"item'$PK_VALUE'\"}"}}' 
done
```

Để theo dõi các hoạt động xóa trên bảng DynamoDB của bạn, điều hướng đến tab﻿ **Monitoring** trên console DynamoDB. Trong thiết lập này, lịch trình EventBridge gọi Lambda function mỗi 5 phút để lấy và xóa mục bằng lệnh﻿ BatchWriteItem. Bạn có thể theo dõi các hoạt động xóa bằng cách xem metric﻿ SuccessfulRequestLatency cho hoạt động﻿ BatchWriteItem, sử dụng thống kê﻿ **Sample Count** để xem số lần gọi xóa. Để biết thêm chi tiết về metrics DynamoDB, tham khảo﻿ [DynamoDB Metrics and dimensions](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/metrics-dimensions.html).

Biểu đồ sau cho thấy﻿ BatchWriteItem được gọi bốn lần, một lần gọi cho mỗi shard được định nghĩa cho trường hợp sử dụng của chúng tôi﻿.

![][image12]

## **Cân nhắc chi phí﻿**

Chi phí sử dụng phương pháp này cho 1.000.000 mục TTL được ước tính trong bảng sau với so sánh sử dụng chức năng DynamoDB gốc. Mỗi mục DynamoDB nhỏ hơn 1KB và được lưu trữ trong bảng sử dụng chế độ on-demand ở Region us-east-1. Free tier không được xem xét cho phân tích này.

|  | TTL gần thời gian thực | TTL DynamoDB bản địa |
| :---- | :---- | :---- |
| DynamoDB global secondary index | Viết: $0.63 Đọc: $0.11 | \- |
| Lambda |  |  |
| EventBridge Scheduler | $1 | \- |
| DynamoDB Delete | $1.26 | \- |
| **Tổng Chi phí** | $2.43 | $0 |

## **Dọn dẹp﻿**

Nếu bạn tạo môi trường kiểm tra để theo dõi bài đăng này, hãy đảm bảo﻿:

1. [Xóa DynamoDB table﻿](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/getting-started-step-6.html)

2. [Xóa Lambda function﻿](https://docs.aws.amazon.com/lambda/latest/dg/urls-webhook-tutorial.html#urls-webhook-tutorial-cleanup)

3. [Xóa EventBridge schedule﻿](https://docs.aws.amazon.com/scheduler/latest/UserGuide/managing-schedule-delete.html)

4. [Xóa bất kỳ IAM role nào còn lại được tạo trong quá trình này](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_delete.html)﻿

5. Xóa bất kỳ tài nguyên nào khác bạn tạo để kiểm tra giải pháp﻿.

## **Tóm tắt﻿**

Trong bài đăng này, chúng tôi đã thảo luận về cách DynamoDB GSI kết hợp với EventBridge và Lambda cung cấp giải pháp mạnh mẽ để quản lý hết hạn dữ liệu trong thời gian thực, cung cấp hiệu suất ứng dụng tối ưu và xử lý dữ liệu hiệu quả.

Trong﻿ [Phần 3﻿](https://aws.amazon.com/blogs/database/implement-event-driven-architectures-with-amazon-dynamodb-part-3/), chúng tôi khám phá cách EventBridge Scheduler có thể cho phép lập lịch chi tiết các sự kiện downstream. Phương pháp này cung cấp quản lý dữ liệu tương lai chính xác cho ứng dụng của bạn, nâng cao kiến trúc hướng sự kiện của bạn.

---

## **Về các tác giả﻿**

| ![][image13] Lee Hannigan [Lee](https://www.linkedin.com/in/lee-hannigan/) Hannigan là Chuyên gia giải pháp DynamoDB cao cấp (Sr. DynamoDB Specialist Solutions Architect) làm việc tại Donegal, Ireland. Anh có chuyên môn sâu rộng về các hệ thống phân tán (distributed systems), cùng nền tảng vững chắc về các công nghệ dữ liệu lớn (big data) và phân tích (analytics technologies). Trong vai trò Chuyên gia giải pháp DynamoDB, Lee xuất sắc trong việc hỗ trợ khách hàng thiết kế, đánh giá và tối ưu hóa khối lượng công việc (workloads) sử dụng các khả năng của DynamoDB. |
| :---- |

| ![][image14] Aman Dhingra [Aman](https://www.linkedin.com/in/amdhing/) Dhingra là Chuyên gia giải pháp DynamoDB cao cấp (Sr. DynamoDB Specialist Solutions Architect) làm việc tại Dublin, Ireland. Anh có đam mê về các hệ thống phân tán (distributed systems) và nền tảng chuyên sâu về dữ liệu lớn & phân tích (big data & analytics). Aman là tác giả của cuốn "Amazon DynamoDB – The Definitive Guide" và hỗ trợ khách hàng trong việc thiết kế, đánh giá và tối ưu hóa khối lượng công việc vận hành trên Amazon DynamoDB. |
| :---- |

[image1]: /images/3-Blog/Blog-2/image_1.png
[image2]: /images/3-Blog/Blog-2/image_2.png
[image3]: /images/3-Blog/Blog-2/image_3.png
[image4]: /images/3-Blog/Blog-2/image_4.png
[image5]: /images/3-Blog/Blog-2/image_5.png
[image6]: /images/3-Blog/Blog-2/image_6.png
[image7]: /images/3-Blog/Blog-2/image_7.png
[image8]: /images/3-Blog/Blog-2/image_8.png
[image9]: /images/3-Blog/Blog-2/image_9.png
[image10]: /images/3-Blog/Blog-2/image_10.png
[image11]: /images/3-Blog/Blog-2/image_11.png
[image12]: /images/3-Blog/Blog-2/image_12.png
[image13]: /images/3-Blog/Blog-1/image_11.png
[image14]: /images/3-Blog/Blog-1/image_12.png
