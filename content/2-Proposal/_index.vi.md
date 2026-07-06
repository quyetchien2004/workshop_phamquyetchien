---
title: "Bản đề xuất"
date: 2026-04-17
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

Tại phần này, nhóm tóm tắt đề xuất triển khai hệ thống **CCT Hotels Booking** lên AWS theo hướng chi phí thấp, dễ demo và phù hợp cho báo cáo thực tập.

# CCT Hotels Booking trên AWS
## Kiến trúc chi phí thấp cho hệ thống đặt phòng khách sạn, thanh toán và gửi email

### 1. Tổng quan dự án
CCT Hotels Booking là hệ thống web hỗ trợ khách hàng và quản trị viên trong quy trình đặt phòng khách sạn. Hệ thống cho phép người dùng xem phòng, tìm kiếm phòng trống, đăng ký/đăng nhập, tạo đặt phòng, thanh toán qua VNPay sandbox và nhận email thông báo như mã OTP, đặt lại mật khẩu hoặc xác nhận đặt phòng.

Dự án được triển khai lên AWS theo mô hình phù hợp cho báo cáo thực tập: rõ kiến trúc, dễ giải thích, có đủ frontend, backend, cơ sở dữ liệu, email, thanh toán và bảo mật cơ bản. Frontend React/Vite được build thành static files và lưu trên Amazon S3, sau đó phân phối qua Amazon CloudFront. Backend Node.js/Express và Socket.IO chạy trên Elastic Beanstalk. Dữ liệu được lưu trong Amazon DocumentDB, email gửi qua Amazon SES, còn VNPay là cổng thanh toán bên thứ ba.

### 2. Mục tiêu
- Triển khai hệ thống CCT Hotels Booking lên AWS để có thể truy cập và demo qua Internet.
- Sử dụng các dịch vụ AWS phù hợp với đồ án thực tập, chi phí có thể kiểm soát nhưng vẫn thể hiện được kiến trúc thực tế.
- Tách rõ các lớp: frontend, backend, database, email, thanh toán và bảo mật.
- Cải thiện hiệu năng và độ an toàn bằng CloudFront, HTTPS, WAF, private subnet và security group.
- Hoàn thiện luồng demo: truy cập web, đăng ký/đăng nhập, tìm phòng, đặt phòng, nhận email và kiểm thử thanh toán VNPay sandbox.
- Tạo tài liệu kiến trúc để anh/chị hướng dẫn hoặc admin dễ theo dõi và nhận xét.

### 3. Vấn đề cần giải quyết
Khi chạy ở môi trường local, hệ thống gặp một số hạn chế:

- Website chỉ chạy trên máy cá nhân, người khác không thể truy cập ổn định từ bên ngoài.
- Frontend chưa có CDN để tối ưu tốc độ tải và phân phối nội dung.
- Backend, database, email và payment đang phụ thuộc nhiều vào cấu hình local.
- VNPay callback/IPN cần endpoint public nên khó kiểm thử nếu chỉ chạy localhost.
- Gửi email bằng SMTP cá nhân không phù hợp để trình bày trong kiến trúc cloud.
- Database cần được bảo vệ, không nên mở trực tiếp ra Internet.
- Nhóm cần có kiến trúc rõ ràng về bảo mật, giám sát và kiểm soát chi phí trên AWS.

Giải pháp AWS giúp tách từng thành phần ra đúng dịch vụ, chỉ public những phần cần thiết và giữ database trong mạng riêng.

### 4. Kiến trúc giải pháp
Kiến trúc được thiết kế theo nhiều lớp, gồm lớp truy cập người dùng, lớp phân phối nội dung, lớp backend, lớp database, lớp email và lớp thanh toán bên ngoài.

![CCT Hotels Booking AWS Architecture](/images/2-Proposal/anh%201.jpg)

#### Các dịch vụ AWS sử dụng
- **Amazon S3**: Lưu trữ frontend đã build từ React/Vite và phục vụ static assets.
- **Amazon CloudFront**: CDN phân phối website, hỗ trợ HTTPS, cache và định tuyến API về backend.
- **AWS WAF**: Bảo vệ ứng dụng ở lớp edge, lọc các request độc hại phổ biến.
- **AWS Elastic Beanstalk**: Triển khai backend Node.js/Express trên EC2 và Application Load Balancer.
- **Amazon DocumentDB**: Lưu dữ liệu người dùng, phòng, đặt phòng, thanh toán, OTP và các dữ liệu nghiệp vụ khác theo chuẩn MongoDB-compatible.
- **Amazon SES**: Gửi OTP, email đặt lại mật khẩu và email xác nhận đặt phòng.
- **Amazon Route 53 / Custom Domain**: Quản lý domain và trỏ domain về CloudFront.
- **AWS Certificate Manager (ACM)**: Cấp chứng chỉ HTTPS cho CloudFront/custom domain.
- **Amazon VPC**: Tách public/private subnet, route table, security group, NAT Gateway và Internet Gateway.

#### Luồng hoạt động chính
1. Người dùng truy cập website qua custom domain hoặc CloudFront domain bằng HTTPS.
2. CloudFront lấy và cache các file frontend từ S3 bucket.
3. AWS WAF kiểm tra request trước khi chuyển tiếp vào hệ thống.
4. Các request API dạng `/api/*` và Socket.IO dạng `/socket.io/*` được CloudFront chuyển về backend Elastic Beanstalk.
5. Application Load Balancer phân phối request đến EC2 app server chạy Node.js/Express.
6. Backend kết nối riêng tư đến Amazon DocumentDB để đọc/ghi dữ liệu.
7. DocumentDB lưu dữ liệu tài khoản, phòng, booking, payment và OTP.
8. Backend gửi email thông qua Amazon SES SMTP.
9. Khi thanh toán, backend tạo payment URL và chuyển người dùng sang VNPay sandbox.
10. VNPay gọi lại Return URL/IPN qua CloudFront về backend để cập nhật trạng thái thanh toán.

#### Thiết kế mạng và bảo mật
- Public subnet dùng cho các thành phần cần public như Load Balancer và NAT Gateway.
- Private subnet dùng cho app server và Amazon DocumentDB.
- DocumentDB không mở ra Internet, chỉ cho phép kết nối từ security group của app server.
- App server đi Internet qua NAT Gateway khi cần gọi dịch vụ bên ngoài.
- CloudFront dùng HTTPS với chứng chỉ ACM.
- WAF được gắn ở CloudFront để bảo vệ tầng truy cập đầu tiên.
- Biến môi trường được cấu hình trong Elastic Beanstalk, không commit file `.env` chứa secret lên repository.

### 5. Timeline
| Giai đoạn | Thời gian | Công việc chính | Kết quả cần có |
|---|---:|---|---|
| Planning | Tuần 9 | Họp nhóm, thống nhất chủ đề CCT Hotels Booking, phân chia nhiệm vụ, vẽ sơ đồ kiến trúc trên draw.io | Chủ đề project, sơ đồ kiến trúc AWS, phân công thành viên |
| Network | Tuần 10 | Tạo VPC, public/private subnet, route table, Internet Gateway, NAT Gateway và security group chính | Môi trường network sẵn sàng cho frontend, backend và database |
| Backend / Database | Tuần 10 - Tuần 11 | Deploy backend Node.js/Express, cấu hình ALB, target group, health check, kết nối Amazon DocumentDB | Backend URL, API health check, kết nối database hoạt động |
| Frontend / CDN | Tuần 10 - Tuần 11 | Build frontend, upload lên S3, tạo CloudFront, gắn WAF/HTTPS/domain và cấu hình behavior API | Website public, CloudFront routing, HTTPS hoạt động |
| Integration / Test | Tuần 11 | Test đăng ký/đăng nhập, tìm phòng, đặt phòng, email, VNPay sandbox và luồng frontend gọi API | Project chạy được luồng chính, có ảnh minh chứng và nội dung báo cáo |

### 6. Ngân sách dự kiến
Dự án ưu tiên chi phí thấp để phục vụ demo và báo cáo thực tập. Chi phí thực tế phụ thuộc vào thời gian bật tài nguyên, region, traffic và việc nhóm có tắt/xóa tài nguyên sau khi demo hay không.

| Thành phần | Ghi chú chi phí |
|---|---|
| Amazon S3 | Chi phí thấp cho lưu trữ static frontend |
| Amazon CloudFront | Thấp với traffic demo, có thể nằm trong mức miễn phí tùy điều kiện tài khoản |
| AWS WAF | Có chi phí theo Web ACL/rule/request nếu bật |
| Elastic Beanstalk / EC2 | Phụ thuộc loại instance và thời gian chạy |
| Application Load Balancer | Có chi phí theo giờ và lượng traffic |
| Amazon DocumentDB | Là thành phần tốn chi phí chính nếu chạy liên tục |
| NAT Gateway | Có thể tốn chi phí nếu để chạy lâu |
| Amazon SES | Rất thấp với số lượng email nhỏ |
| Route 53 / Domain | Có chi phí domain và hosted zone nếu sử dụng |

Với demo ngắn hạn, nhóm có thể kiểm soát chi phí bằng cách tắt hoặc xóa tài nguyên không cần thiết sau khi test, đặc biệt là DocumentDB, Elastic Beanstalk, Load Balancer và NAT Gateway. Nếu để toàn bộ tài nguyên chạy cả tháng, chi phí có thể tăng đáng kể. Nhóm nên bật AWS Budgets để nhận cảnh báo khi chi phí vượt ngưỡng dự kiến.

### 7. Rủi ro
| Rủi ro | Mức ảnh hưởng | Cách giảm thiểu |
|---|---|---|
| Phát sinh chi phí AWS ngoài dự kiến | Cao | Tạo AWS Budgets, tắt DocumentDB/Elastic Beanstalk/NAT Gateway sau demo, theo dõi Billing Dashboard |
| SES còn ở sandbox | Trung bình | Verify email người gửi/người nhận hoặc request production access nếu cần gửi cho email bất kỳ |
| VNPay sandbox báo website chưa được phê duyệt | Trung bình | Kiểm tra đúng TM Code, Hash Secret, Return URL, IPN URL và liên hệ VNPay nếu cần duyệt website |
| Lỗi CORS sau khi deploy | Trung bình | Cấu hình đúng `CLIENT_URL` và `SERVER_URL` theo CloudFront/custom domain |
| CloudFront cache file cũ | Thấp | Tạo invalidation `/*` sau mỗi lần upload frontend mới |
| Backend không kết nối được DocumentDB | Cao | Kiểm tra VPC, subnet group, security group, TLS CA file và MongoDB URI |
| Sai security group | Cao | Chỉ mở đúng port cần thiết, giới hạn DocumentDB chỉ nhận traffic từ app security group |
| Lộ secret trong repository hoặc ảnh chụp | Cao | Lưu secret trong environment variables, không commit `.env`, xoay vòng credential nếu bị lộ |
| Kiến trúc demo chỉ dùng ít instance | Trung bình | Chấp nhận để tối ưu chi phí demo hoặc mở rộng Auto Scaling/replica khi cần high availability |

### 8. Kết quả mong đợi
Sau khi hoàn thành, hệ thống CCT Hotels Booking có thể chạy trên AWS với frontend public qua CloudFront/S3, backend public qua Elastic Beanstalk, database được bảo vệ trong private subnet, email gửi qua SES và thanh toán thử nghiệm qua VNPay sandbox. Kiến trúc này phù hợp để trình bày trong báo cáo thực tập vì thể hiện được quy trình triển khai thực tế, phân tách dịch vụ, bảo mật, vận hành và kiểm soát chi phí.
