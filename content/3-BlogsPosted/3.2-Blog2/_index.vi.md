---
title: "Triển khai AWS Organizations bằng CloudFormation"
date: 2026-04-17
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# [AWS Organizations] Triển khai AWS Organizations bằng CloudFormation

![AWS Organizations bằng CloudFormation](/images/blog/blog2.png)

Chào anh em AWS Study Group VN!

AWS đã chính thức hỗ trợ AWS Organizations trên CloudFormation, mở ra khả năng quản lý toàn bộ cấu trúc tổ chức theo mô hình Infrastructure as Code (IaC). Đây là một cập nhật đáng chú ý đối với các doanh nghiệp đang vận hành môi trường AWS nhiều tài khoản.

Trước đây, việc tạo Organizational Unit (OU), tài khoản AWS hay các chính sách quản trị trong AWS Organizations thường được thực hiện thủ công thông qua AWS Console hoặc AWS CLI. Khi số lượng tài khoản tăng lên, việc quản lý trở nên phức tạp, tốn thời gian và khó đảm bảo tính nhất quán giữa các môi trường.

Với CloudFormation, người dùng có thể định nghĩa và triển khai các tài nguyên của AWS Organizations thông qua template. Từ đó, team có thể tạo mới, cập nhật hoặc xóa cấu trúc tổ chức một cách tự động và dễ kiểm soát hơn.

## Các resource type được hỗ trợ

Hiện tại CloudFormation hỗ trợ ba resource type chính:

* `AWS::Organizations::Account`: tạo tài khoản AWS mới và tự động đưa vào Organization.
* `AWS::Organizations::OrganizationalUnit`: tạo OU trong Root hoặc trong OU cha.
* `AWS::Organizations::Policy`: tạo và quản lý các loại chính sách như Service Control Policy (SCP), Tag Policy, Backup Policy và AI Services Opt-Out Policy.

## Ví dụ triển khai

Trong ví dụ được AWS giới thiệu, một template có thể tự động tạo các OU như Infrastructure, Production và Security. Template cũng có thể tạo tài khoản AccountA, đồng thời triển khai các SCP để ngăn tài khoản rời khỏi Organization hoặc ngăn việc vô hiệu hóa CloudTrail.

Ngoài ra, Tag Policy cũng có thể được áp dụng để chuẩn hóa cách đặt tag trên toàn bộ môi trường AWS. Điều này giúp các team dễ quản lý tài nguyên hơn, đặc biệt khi số lượng account và workload tăng lên.

## Lợi ích

Lợi ích lớn nhất của tính năng này là giúp doanh nghiệp quản lý AWS Organizations theo mã nguồn. Cấu trúc tổ chức có thể được lưu trong Git, review trước khi thay đổi và tích hợp vào các quy trình tự động hóa.

Nhờ đó, team có thể mở rộng hoặc thay đổi cấu trúc hiện có nhanh hơn, triển khai môi trường mới nhất quán hơn và áp dụng chính sách đồng nhất trên nhiều tài khoản AWS.

## Kết

Việc AWS Organizations hỗ trợ CloudFormation là một bước tiến quan trọng trong việc chuẩn hóa quản trị AWS ở quy mô lớn. Tính năng này giúp giảm thao tác thủ công, tăng tính nhất quán và hỗ trợ triển khai hạ tầng theo hướng Infrastructure as Code.

Với các môi trường multi-account, đây là một hướng rất đáng quan tâm vì nó giúp quản trị tài khoản AWS rõ ràng và dễ kiểm soát hơn.

**Nguồn tham khảo:** [Deploy AWS Organizations resources by using CloudFormation](https://aws.amazon.com/vi/blogs/security/deploy-aws-organizations-resources-by-using-cloudformation/)
