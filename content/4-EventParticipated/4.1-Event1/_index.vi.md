---
title: "FCAJ Community Day - Conference Call"
date: 2026-05-23
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Event 1: FCAJ Community Day - Conference Call

## Thông tin sự kiện

* **Tên sự kiện:** FCAJ Community Day - Conference Call
* **Thời gian:** 09:00 - 12:00, Thứ Bảy ngày 23/05/2026
* **Địa điểm:** Bitexco Financial Tower, Thành phố Hồ Chí Minh
* **Vai trò:** Người tham gia

## Nội dung chính của sự kiện

Trong sự kiện này, em tham gia với vai trò người nghe và ghi nhận lại các nội dung được chia sẻ từ các anh chị diễn giả trong cộng đồng AWS First Cloud AI Journey. Sự kiện tập trung vào các chủ đề liên quan đến AWS, AI, hệ thống production và cách áp dụng công nghệ vào các bài toán thực tế.

Các nội dung chính em ghi nhận được từ các phần thuyết trình:

### CloudFront as Your Foundation

Phần trình bày của anh Nguyễn Tuấn Thịnh giới thiệu vai trò của Amazon CloudFront trong việc phân phối nội dung từ edge đến origin. Nội dung nhấn mạnh CloudFront không chỉ dùng để cache nội dung, mà còn hỗ trợ bảo mật, tối ưu chi phí, tăng hiệu năng và cải thiện khả năng chịu lỗi.

Một số ý chính:

* CloudFront sử dụng mạng lưới edge location toàn cầu để giảm độ trễ cho người dùng.
* Có thể kết hợp CloudFront với AWS WAF, AWS Shield, Route 53 và S3 để tăng bảo mật.
* CloudFront giúp giảm tải cho origin, giảm CPU usage và giảm chi phí cho Load Balancer.
* Các tính năng như Origin Access Control, signed URL, geographic restriction và origin failover giúp bảo vệ và tăng độ ổn định cho hệ thống.

### Context Is Everything

Phần chia sẻ của anh Tịnh Trương nói về cách sử dụng AI hiệu quả hơn thông qua context. Em hiểu được rằng nhiều câu trả lời AI chưa tốt không hẳn do model yếu, mà do người dùng chưa cung cấp đủ mục tiêu, ràng buộc và thông tin liên quan.

Một số ý chính:

* Context giúp AI hiểu đúng nhiệm vụ cần làm.
* Không phải cứ đưa thật nhiều thông tin là tốt, quan trọng là đưa đúng thông tin.
* Một prompt tốt nên có mục tiêu, dữ liệu liên quan, ràng buộc và tiêu chí đánh giá kết quả.
* Context engineering có thể trở thành một kỹ năng quan trọng khi làm việc với AI trong tương lai.

### Friendly AI Assistant with Amazon Quick

Phần trình bày của anh Hải Anh giới thiệu Amazon Quick như một hướng tiếp cận để xây dựng trợ lý AI thân thiện cho người dùng doanh nghiệp. Nội dung tập trung vào việc giúp người dùng tổng hợp thông tin từ nhiều nguồn, tự động hóa các tác vụ lặp lại và hỗ trợ làm việc hiệu quả hơn.

Một số use case được nhắc đến:

* Tự động tạo biên bản cuộc họp.
* Gửi email cho các bên liên quan.
* Lên lịch cuộc họp tiếp theo.
* Kết nối dữ liệu, dashboard, workflow và các công cụ AI trong một trải nghiệm thống nhất.

### Enterprise-Grade Multi-Agent System

Phần trình bày của chị Vy Lam nói về hệ thống multi-agent trong bài toán startup credit scoring. Em thấy chủ đề này khá thực tế vì một single agent khó xử lý tốt các bài toán có nhiều góc nhìn như tài chính, thị trường, đội ngũ sáng lập, rủi ro và tuân thủ.

Một số ý chính:

* Multi-agent system có thể hoạt động giống một hội đồng đánh giá ảo.
* Mỗi agent phụ trách một mảng chuyên môn riêng như financial analyst, market analyst, team evaluator, risk assessor hoặc compliance.
* Hệ thống enterprise-grade cần quan tâm đến security, data governance, network, operations, human factors và compliance.
* Guardrails rất quan trọng để kiểm soát input, quá trình xử lý và output của hệ thống AI.

### Non-Determinism of "Deterministic" LLM Settings

Phần trình bày của anh Đào Đức giúp em hiểu thêm rằng ngay cả khi đặt temperature bằng 0, kết quả từ LLM vẫn có thể không hoàn toàn giống nhau trong mọi lần chạy. Nguyên nhân đến từ cách GPU xử lý floating-point, batching khi inference và các tối ưu hạ tầng phía sau.

Một số ý chính:

* Temperature bằng 0 chỉ loại bỏ sampling randomness, không loại bỏ hết mọi nguồn gây khác biệt.
* Với các hệ thống quan trọng như y tế, pháp lý hoặc tài chính, cần thiết kế để xử lý sự biến thiên của output.
* Có thể giảm rủi ro bằng cách chạy nhiều lần, dùng majority voting, structured output hoặc kiểm thử kỹ hơn.
* Khi xây dựng ứng dụng AI, không nên xem output của LLM là tuyệt đối cố định.

### UTMorpho - LotusHacks 2026

Phần chia sẻ của Team VIB nói về hành trình tham gia LotusHacks 2026 và xây dựng sản phẩm UTMorpho trong 36 giờ. Nội dung không đi quá sâu vào một dịch vụ AWS cụ thể, nhưng giúp em hiểu thêm về quá trình biến một ý tưởng thành sản phẩm trong thời gian ngắn.

Một số ý chính:

* Hackathon giúp rèn luyện khả năng làm việc dưới áp lực thời gian.
* Khi xây dựng sản phẩm, cần tập trung vào vấn đề thật thay vì cố làm quá nhiều ý tưởng cùng lúc.
* Team sync và phân chia công việc rõ ràng rất quan trọng.
* Việc demo, pitch và kể lại hành trình sản phẩm cũng là một kỹ năng cần luyện tập.

## Bài học rút ra

Sau khi tham gia sự kiện, em rút ra một số bài học chính:

* Khi triển khai hệ thống trên AWS, cần quan tâm song song đến hiệu năng, bảo mật, chi phí và khả năng phục hồi.
* Với AI, chất lượng context ảnh hưởng rất lớn đến chất lượng kết quả.
* Các hệ thống AI dùng trong doanh nghiệp không chỉ cần chạy được, mà còn cần có guardrails, logging, bảo mật và khả năng kiểm soát.
* Khi dùng LLM trong sản phẩm thật, cần chấp nhận rằng output có thể thay đổi và phải thiết kế hệ thống để xử lý điều đó.
* Việc tham gia event giúp em học thêm từ kinh nghiệm thực tế của các anh chị trong cộng đồng, không chỉ từ tài liệu hoặc bài lab.

## Đóng góp cá nhân

Trong event này, em tham gia với vai trò người nghe. Em theo dõi các phần trình bày, đọc lại slide của các diễn giả sau sự kiện và ghi chú các nội dung liên quan đến AWS, AI và cách thiết kế hệ thống. Những ghi chú này giúp em bổ sung thêm kiến thức cho báo cáo thực tập và định hướng tốt hơn cho project cá nhân trong chương trình FCAJ.

## Hình ảnh minh chứng

![FCAJ Community Day - Event 1](/images/event/even1.jpg)
