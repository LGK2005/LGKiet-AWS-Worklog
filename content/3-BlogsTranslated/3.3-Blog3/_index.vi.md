---
title: "Blog 3"
date: "2000-01-01"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
# **Triển khai kiến trúc hướng sự kiện với Amazon DynamoDB – Phần 3**

bởi Lee Hannigan và Aman Dhingra | vào ngày 25 tháng 9 năm 2025 | trong [Advanced (300)](https://aws.amazon.com/blogs/database/category/learning-levels/advanced-300/), [Amazon DynamoDB](https://aws.amazon.com/blogs/database/category/database/amazon-dynamodb/), [Amazon EventBridge](https://aws.amazon.com/blogs/database/category/application-integration/amazon-eventbridge/), [AWS Lambda](https://aws.amazon.com/blogs/database/category/compute/aws-lambda/), [Serverless](https://aws.amazon.com/blogs/database/category/serverless/), [Technical How-to](https://aws.amazon.com/blogs/database/category/post-types/technical-how-to/)

Ở [Phần 1](https://aws.amazon.com/blogs/database/implement-event-driven-architectures-with-amazon-dynamodb/), chúng ta đã tìm hiểu cách sử dụng Amazon EventBridge Scheduler để xóa dữ liệu tự động trong Amazon DynamoDB một cách chính xác. [Phần 2](https://aws.amazon.com/blogs/database/implement-event-driven-architectures-with-amazon-dynamodb-part-2/) đã thảo luận cách sử dụng global secondary index (GSI) để quản lý dữ liệu nghiêm ngặt trong DynamoDB. Bài viết này tập trung vào việc sử dụng EventBridge Scheduler để lập lịch sự kiện chi tiết dựa trên dữ liệu ghi vào DynamoDB.

Trong toàn bộ chuỗi bài, chúng tôi kiểm tra các chiến lược quản lý dữ liệu trong DynamoDB. Bài này chuyển sang mẫu hướng sự kiện để lên lịch hành động downstream tin cậy trong tương lai bằng EventBridge Scheduler. Một lợi thế của cách tiếp cận này là khả năng kích hoạt các sự kiện downstream quan trọng về thời gian, như gửi thông báo nhắc nhở các cuộc hẹn, hết hạn ưu đãi, hoặc gia hạn đăng ký. Ví dụ, khi đăng ký của người dùng sắp hết hạn, EventBridge Scheduler có thể kích hoạt một sự kiện gọi [AWS Lambda](https://aws.amazon.com/lambda/) kèm chi tiết item DynamoDB liên quan. Lambda này sau đó có thể dùng [Amazon Simple Email Service](https://aws.amazon.com/ses/) (Amazon SES) để gửi thông báo kịp thời tới người dùng.

Kiến trúc này giúp người dùng nhận được nhắc nhở đúng lúc, cải thiện trải nghiệm và tăng mức độ tương tác. EventBridge Scheduler linh hoạt cho phép kiểm soát chính xác thời gian thông báo, đáp ứng nhiều bài toán kinh doanh khác nhau.

## **Tổng quan giải pháp**

Giải pháp này trình diễn cách sử dụng [Amazon DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) và AWS Lambda để tự động lên lịch hành động trong tương lai dựa trên việc ghi item vào DynamoDB. Bằng cách nhận stream records từ các lần ghi, một hàm Lambda được kích hoạt để tạo các lịch chi tiết theo thời gian qua Amazon EventBridge Scheduler. Những lịch này sau đó có thể gọi các dịch vụ downstream như Amazon SES để gửi email nhắc nhở, [Amazon Simple Queue Service](https://aws.amazon.com/sqs/) (Amazon SQS), [AWS Step Functions](https://aws.amazon.com/step-functions/) hoặc các dịch vụ AWS khác, cho phép quy trình hướng sự kiện có tính mở rộng, tin cậy cao.

Sơ đồ sau minh họa kiến trúc giải pháp.

![][image1]

Một trường hợp phổ biến cho mẫu này là lập lịch các sự kiện trong tương lai như nhắc nhở cuộc hẹn, hết hạn ưu đãi, hoặc gia hạn đăng ký với độ tin cậy cao. EventBridge Scheduler cho phép lập lịch hành động một lần hoặc lặp lại dựa trên thuộc tính lưu trong các item DynamoDB. Luồng hoạt động như sau:

1. Ghi dữ liệu – Khi một item được ghi lên DynamoDB table, một bản ghi stream ứng sẽ được tạo trong DynamoDB stream được liên kết của nó.

2. Kích hoạt – Bản ghi stream này, riêng lẻ hoặc trong batch, sẽ kích hoạt một AWS Lambda function.

3. Tạo lịch – Lambda function dùng EventBridge Scheduler để lên lịch trong tương lai. Lịch bao gồm timestamp chính xác và dữ liệu item liên quan.

4. Kích hoạt mục tiêu – Đến thời gian đã định, EventBridge Scheduler gọi mục tiêu cấu hình, ví dụ gửi email với Amazon SES, đưa message vào SQS, hoặc bắt đầu thực thi Step Functions.

Kiến trúc này được thiết kế để mở rộng theo dữ liệu, cho phép lập lịch logic chính xác mà không làm phức tạp mô hình dữ liệu DynamoDB hoặc cần polling liên tục. Hãy tham khảo các bước áp dụng giải pháp.

## **Yêu cầu trước**

Trước khi áp dụng giải pháp hướng sự kiện, bạn cần phải có các yêu cầu sau:

* **Tài khoản AWS** – Truy cập một [tài khoản AWS](https://aws.amazon.com/free) đang hoạt động.

* **Kiến thức cơ bản về DynamoDB** – Hiểu về [khái niệm DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html): bảng, item, thuộc tính, thao tác CRUD cơ bản là cần thiết để cấu hình và quản lý cơ sở dữ liệu một cách hiệu quả.

* **Lambda functions** – Thành thạo Lambda vì bạn sẽ tạo và triển khai Lambda function để xử lý sự kiện [Amazon DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) và tạo lịch qua EventBridge Scheduler.

* **EventBridge Scheduler** – Kiến thức cơ bản về [Amazon EventBridge](https://aws.amazon.com/eventbridge/) là cần thiết cho việc cấu hình EventBridge Scheduler rule để gọi API cụ thể.

* **Amazon SES** – Kiến thức cơ bản về [Amazon SES](https://docs.aws.amazon.com/ses/latest/dg/Welcome.html) và xác thực email người gửi. Để tạo và xác thực địa chỉ email, xem tại [Creating and verifying identities in Amazon SES](https://docs.aws.amazon.com/ses/latest/dg/creating-identities.html).

* **Thành thạo AWS CLI hoặc console** – Thành thạo dùng [AWS Command Line Interface](https://aws.amazon.com/cli/) (AWS CLI) hoặc [AWS Management Console](https://aws.amazon.com/console/) cho việc cấu hình dịch vụ, tạo resource và giám sát log.

## **Tạo bảng DynamoDB**

Hoàn thành các bước sau để tạo DynamoDB table:

1. Ở DynamoDB console, chọn **Tables** ở thanh điều hướng.

2. Chọn **Create table**.

3. Vói **tên Table**, nhập tên table của bạn.

4. Với **Partition key**, nhập PK làm tên, chọn kiểu **String** làm kiểu dữ liệu.

5. Với **Sort key**, nhập SK làm tên, chọn kiểu **String** làm kiểu dữ liệu.

![][image2]

6. Để mặc định các cấu hình khác, chọn **Create table**. Bảng sẽ tạo sau vài giây.

7. Vào lại **Tables**, mở bảng vừa tạo.

8. Trong mục **DynamoDB stream details**, chọn **Turn on**.

![][image3]

9. Chọn **New and old images**, sau đó chọn **Turn on stream**.

![][image4]

Điều này sẽ cho phép Luồng DynamoDB trên bảng hiển thị cả trạng thái cũ và mới của các mục trong bản ghi stream, để bạn có thể quản lý các cập nhật giá trị Time to Live (TTL) của các item.

## **Tạo Lambda function**

Hoàn thành các bước sau để tạo Lambda function:

1. Vào console Lambda, chọn **Functions**.

2. Chọn **Create function**.

3. Chọn **Author from scratch**.

4. Đối với **Function name**, hãy nhập tên (ví dụ: DDBStreamTriggerEventScheduler).

5. Chọn **Runtime** Node.js mới nhất.

6. Đối với vai trò dịch vụ Lambda mà bạn đã gắn vào function, hãy thêm chính sách được quản lý [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM) AWSLambdaDynamoDBExecutionRole và một chính sách nội tuyến có quyền lên lịch: CreateSchedule.

7. Chọn **Create function**.

![][image5]

8. Sau khi tạo Lambda function, chọn **Add trigger** để cấu hình event source mapping cho bảng DynamoDB.

9. Chọn **DynamoDB** làm source.

10. Đối với **DynamoDB table**, hãy nhập ARN cho Appointment-Table.

11. Để mặc định các cấu hình còn lại, chọn **Add** để tạo trigger.

![][image6]

12. Ở tab **Code** của Lambda function, thay code mặc định bằng đoạn mã Node.js sau. Đảm bảo cập nhật phần giữ chỗ với các giá trị thích hợp—chẳng hạn như thay thế ses-verified-email@example.com bằng địa chỉ email Amazon SES đã được xác minh của bạn. Ngoài ra, hãy đảm bảo rằng vai trò IAM mà EventBridge Scheduler sử dụng có quyền ses:SendEmail.

| import { SchedulerClient, CreateScheduleCommand } from "@aws-sdk/client-scheduler";
const client \= new SchedulerClient({ region: \> }); // vd: eu-west-1
export const handler \= async (event) \=\> {
  try {
    for (const record of event.Records) {
      let params \= {
        eventID: record.eventID,
        sequenceNumber: record.dynamodb.SequenceNumber,
        email: record.dynamodb.email,           // Email gửi
        subject: "Time for your appointment",   // Chủ đề email
        reminderTS: record.dynamodb.NewImage.REMINDER\_TIMESTAMP.S, // Thời gian nhắc nhở (ISO)
      };
      params.body \= "This is the email body, you have a reminder"; // Nội dung
      await scheduleEmail(params);
    }
    const response \= {
      statusCode: 200,
      body: JSON.stringify('Complete'),
    };
    return response;
  } catch (error) {
    console.error("Error processing event: ", error);
    const response \= {
      statusCode: 500,
      body: JSON.stringify({ message: 'Error processing event', error: error.message }),
    };
    return response;
  }
};
const scheduleEmail \= async (params) \=\> {
  try {
    const sesParams \= {
      Destination: { ToAddresses: \[params.email\] },
      Message: {
        Body: { Text: { Data: params.body } },
        Subject: { Data: params.subject },
      },
      Source: "ses-verified-email@example.com", // Email gửi xác thực
    };
    const target \= {
      RoleArn: \>,    // vd: arn:aws:iam::XXXX:role/SchedulerRole
      Arn: \>,        // vd: "arn:aws:scheduler:::aws-sdk:ses:sendEmail",
      Input: JSON.stringify(sesParams),
      DeadLetterConfig: { Arn: \> } // vd: arn:aws:sqs:eu-west-1:XXXX:Appointment-DLQ
    };
    const schedulerInput \= {
      Name: params.eventID,
      FlexibleTimeWindow: { Mode: "OFF" },
      ActionAfterCompletion: "DELETE",
      Target: target,
      ScheduleExpression: \`at(${params.reminderTS})\`,
      ClientToken: params.sequenceNumber,
    };
    const command \= new CreateScheduleCommand(schedulerInput);
    const result \= await client.send(command);
    return result;
  } catch (error) {
    console.error("Error scheduling email: ", error);
    throw new Error(\`Failed to schedule email: ${error.message}\`);
  }
}; |
| :---- |

13. Chọn **Deploy** để triển khai code mới nhất.

Mẹo: Để cải thiện độ tin cậy và khả năng theo dõi, bạn có thể định cấu hình [Dead Letter Queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html) (DLQ) tại hai điểm trong kiến ​​trúc này. Trước tiên, bạn có thể thêm DLQ vào Lambda function sử dụng từ DynamoDB stream, thao tác này ghi lại mọi lỗi xảy ra trong khi xử lý bản ghi stream hoặc tạo lịch trình, chẳng hạn như lỗi đầu vào hoặc lỗi quyền hạn. Thứ hai, bạn có thể định cấu hình DLQ như một phần của EventBridge Scheduler target. Điều này ghi lại các lỗi xảy ra tại thời điểm thực thi, chẳng hạn như nếu Amazon SES không gửi được email hoặc dịch vụ đích không khả dụng. Việc sử dụng cả hai DLQ cho phép bạn theo dõi, phân tích và thử lại các lỗi trong toàn bộ vòng đời lập kế hoạch và phân phối.

## **Sinh dữ liệu mẫu để kiểm tra giải pháp**

Chạy lệnh AWS CLI sau để mô phỏng hoạt động ghi vào DynamoDB table của bạn. Vòng lặp này chèn 10 items mẫu vào bảng, mỗi mục có một khóa phân vùng duy nhất (PK) và một khóa sắp xếp tĩnh (SK). Mỗi mục bao gồm REMINDER\_TIMESTAMP được đặt thành 3 phút kể từ thời điểm hiện tại và địa chỉ email kiểm tra. Những lần ghi này sẽ kích hoạt DynamoDB Stream, luồng này gọi Lambda function của bạn để lên lịch gửi email nhắc nhở thông qua EventBridge Scheduler. Hãy nhớ thay thế abc@example.com bằng địa chỉ email hợp lệ, đã được xác minh trong Amazon SES để quan sát toàn bộ quy trình của giải pháp.

| \#\!/bin/bash
TABLE="Appointment-Table"
for PK\_VALUE in {1..10} ; do
  ISO\_TIMESTAMP\_PLUS\_3\_MINS=$(date \-v+3M \-u \+"%Y-%m-%dT%H:%M:%S")
  aws dynamodb put-item \--table-name $TABLE \\
    \--item '{"PK": {"S": "'$PK\_VALUE'"}, "SK": {"S": "StaticSK"}, "REMINDER\_TIMESTAMP": {"S": "'$ISO\_TIMESTAMP\_PLUS\_3\_MINS'"}, "email": {"S": "abc@example.com"}, "ATTR\_1": {"S": "This is a static attribute"}}'
done |
| :---- |

Để theo dõi các email nhắc nhở đang được gửi, hãy điều hướng đến **Monitoring** tab của EventBridge schedule group. Bạn có thể xem các sự kiện được EventBridge Scheduler gọi vào một thời điểm cụ thể bằng cách xem số liệu InvocationAttemptCount. Trong trường hợp của chúng ta, lời gọi là các email nhắc nhở cuộc hẹn tới người dùng thông qua Amazon SES. Để có danh sách tất cả số liệu có sẵn cho một schedule group, tham khảo [Monitoring Amazon EventBridge Scheduler with Amazon CloudWatch](https://docs.aws.amazon.com/scheduler/latest/UserGuide/monitoring-cloudwatch.html).

![][image7]

## **Dọn dẹp**

Nếu bạn dựng môi trường test theo bài viết, nhớ [xoá DynamoDB table](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithTables.Basics.html#WorkingWithTables.Basics.DeleteTable), [Lambda function](https://docs.aws.amazon.com/cli/latest/reference/lambda/delete-function.html), [EventBridge schedule](https://docs.aws.amazon.com/scheduler/latest/UserGuide/managing-schedule-delete.html) và các resource khác đã tạo để tiết kiệm chi phí.

## **Tóm tắt**

Trong bài viết này, chúng ta đã được trình bày cách dùng EventBridge Scheduler để lập lịch sự kiện chi tiết dựa trên dữ liệu ghi ở DynamoDB. Giải pháp này cho phép bạn thực hiện các hành động chính xác, kịp thời dựa trên dấu thời gian được lưu trữ trong DynamoDB items khi bạn muốn gửi thông báo có giới hạn thời gian hoặc gọi các downstream jobs.

Trong loạt bài gồm ba phần này, chúng ta đã khám phá cách mở rộng các khả năng gốc của Amazon DynamoDB bằng cách sử dụng các mẫu kiến ​​trúc hướng sự kiện được điều chỉnh cho phù hợp với nhu cầu trong thế giới thực:

* [**Phần 1**](https://aws.amazon.com/blogs/database/implement-event-driven-architectures-with-amazon-dynamodb/): Giới thiệu giải pháp TTL gần real-time, dùng EventBridge Scheduler xóa item quá hạn bằng độ chính xác cao hơn TTL gốc.

* [**Phần 2**](https://aws.amazon.com/blogs/database/implement-event-driven-architectures-with-amazon-dynamodb-part-2/): Trình bày cách xây dựng giải pháp quản lý dữ liệu nghiêm ngặt bằng cách sử dụng sharded global secondary index (GSI), EventBridge Scheduler và Lambda để truy vấn định kỳ và loại bỏ các bản ghi đã hết hạn.

* **Phần 3**: Tập trung vào việc sử dụng DynamoDB Streams và EventBridge Scheduler để lên lịch các hành động xuôi dòng trong tương lai dựa trên dữ liệu được ghi vào DynamoDB table, chẳng hạn như gửi email nhắc nhở cho các cuộc hẹn sắp tới.

Để tìm hiểu sâu hơn và khám phá thêm best practice thiết kế với DynamoDB, EventBridge tại [tài liệu Amazon DynamoDB](https://docs.aws.amazon.com/dynamodb/index.html) và [tài liệu Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-eventbridge.html).

---

**Về các tác giả**

| Lee Hannigan [Lee](https://www.linkedin.com/in/lee-hannigan/) Hannigan là Chuyên gia giải pháp DynamoDB cao cấp (Sr. DynamoDB Specialist Solutions Architect) làm việc tại Donegal, Ireland. Anh có chuyên môn sâu rộng về các hệ thống phân tán (distributed systems), cùng nền tảng vững chắc về các công nghệ dữ liệu lớn (big data) và phân tích (analytics technologies). Trong vai trò Chuyên gia giải pháp DynamoDB, Lee xuất sắc trong việc hỗ trợ khách hàng thiết kế, đánh giá và tối ưu hóa khối lượng công việc (workloads) sử dụng các khả năng của DynamoDB. |
| :---- |

| Aman Dhingra [Aman](https://www.linkedin.com/in/amdhing/) Dhingra là Chuyên gia giải pháp DynamoDB cao cấp (Sr. DynamoDB Specialist Solutions Architect) làm việc tại Dublin, Ireland. Anh có đam mê về các hệ thống phân tán (distributed systems) và nền tảng chuyên sâu về dữ liệu lớn & phân tích (big data & analytics). Aman là tác giả của cuốn "Amazon DynamoDB – The Definitive Guide" và hỗ trợ khách hàng trong việc thiết kế, đánh giá và tối ưu hóa khối lượng công việc vận hành trên Amazon DynamoDB. |
| :---- |

[image1]:

[image2]:

[image3]:

[image4]:

[image5]: 

[image6]:

[image7]: