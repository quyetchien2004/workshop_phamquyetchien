---
title: "Worklog Tuần 8"
date: 2026-06-05
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Kết nối các dịch vụ AWS đã học từ tuần 3 đến tuần 7 vào một mô hình nhỏ.
* Tập liên kết các dịch vụ lại với nhau thay vì học rời rạc.
* Lên ý tưởng cho một mô hình nhỏ gồm website tĩnh, server và database.
* Làm một lab nhỏ liên quan đến VPC endpoint cho S3 để thấy S3 và networking có thể liên kết với nhau.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 6 | - Kết nối AWS Console, IAM và AWS Free Tier vào phần chuẩn bị mô hình <br> - Kiểm tra billing và quyền truy cập trước khi thực hành | 05/06/2026 | 05/06/2026 | |
| 2 | - Tìm hiểu cách dùng S3 trong ứng dụng phần mềm <br> - Xác định phần lưu file hoặc host website tĩnh trong mô hình | 08/06/2026 | 08/06/2026 | <https://docs.aws.amazon.com/s3/> |
| 3 | - Tìm hiểu cách EC2 có thể chạy backend/server <br> - Xác định các thành phần cần có khi triển khai server | 09/06/2026 | 09/06/2026 | <https://docs.aws.amazon.com/ec2/> |
| 4 | - Tìm hiểu cách RDS/DynamoDB và networking kết hợp với nhau <br> - Xác định cách database và server có thể kết nối trong cloud | 10/06/2026 | 10/06/2026 | <https://docs.aws.amazon.com/> |
| 5 | - Làm lab tìm hiểu Gateway endpoint cho Amazon S3 <br> - Thực hiện tìm hiểu cách VPC có thể truy cập S3 | 11/06/2026 | 11/06/2026 | <https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-s3.html> |

### Kết quả đạt được tuần 8:

* Kết nối được các dịch vụ đã học: IAM, S3, EC2, database và VPC.
* Hiểu hơn cách các dịch vụ có thể liên kết với nhau trong một ứng dụng thực tế.
* Biết một mô hình đơn giản có thể gồm S3 để lưu file hoặc host static web, EC2 để chạy server và database để lưu dữ liệu.
* Nhận ra khi triển khai trên cloud cần nghĩ đến nhiều phần cùng lúc: quyền truy cập, network, chi phí và cách xóa tài nguyên.
* Sau bài lab, em hiểu thêm rằng tài nguyên trong VPC có thể truy cập S3 thông qua gateway endpoint thay vì chỉ nghĩ đơn giản là đi qua internet.
* Tuần này giúp em có cái nhìn tổng quát hơn, không chỉ học từng dịch vụ riêng lẻ.
* Em sẽ dùng phần tìm hiểu này để chuẩn bị cho proposal và workshop ở các tuần sau.
