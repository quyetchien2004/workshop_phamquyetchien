---
title: "Kubernetes bị sập cả Region thì sao?"
date: 2026-06-09
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Kubernetes bị sập cả Region thì sao? Đây là cách AWS Backup giúp bạn phục hồi

![Kubernetes Cross-Region DR với AWS Backup](/images/blog/blog1.png)

Nhiều team chạy EKS đã có multi-AZ, nhưng nếu cả một AWS Region gặp sự cố thì multi-AZ không còn đủ để xử lý. Với những hệ thống có yêu cầu RTO/RPO nghiêm ngặt như fintech, ecommerce hoặc healthcare, mình nghĩ cần có một phương án Cross-Region DR rõ ràng hơn.

Bài này mình tóm tắt một cách tiếp cận khá gọn từ AWS: dùng AWS Backup để backup EKS xuyên Region, không cần tự dựng pipeline phức tạp.

## Vấn đề với stateful workloads trên EKS

Backup stateful app như MySQL, Redis, Kafka trên Kubernetes không đơn giản như snapshot một EC2. Mình cần đảm bảo cả Kubernetes manifests và persistent volume data đều nhất quán tại cùng một thời điểm, sau đó replicate toàn bộ sang Region khác.

Trước đây nhiều team thường dùng Velero để xử lý bài toán này. Hiện tại AWS Backup cũng hỗ trợ native cho EKS. Điểm đáng chú ý là nó tạo ra một composite recovery point duy nhất gồm cả Kubernetes resources và EBS snapshots trong một lần backup. Sau đó việc copy sang Region khác có thể thực hiện bằng AWS CLI.

## Luồng hoạt động

1. Deploy EKS ở Region nguồn `us-east-1` với app stateful gồm MySQL 20GB và Redis 10GB chạy trên EBS.
2. Tạo Backup Vault ở cả hai Region, sau đó chạy backup job để tạo composite recovery point.
3. Copy recovery point sang DR Region `us-west-2`.
4. Pre-provision DR cluster sẵn. Đây là điểm quan trọng để giảm RTO vì cluster đã sẵn sàng khi cần restore.
5. Restore toàn bộ app và persistent data vào DR cluster, sau đó kiểm tra dữ liệu MySQL và Redis có khớp với source hay không.

Sau khi restore thành công, hệ thống có 6 pods running, 2 PVCs bound và Load Balancer URL active. Như vậy app có thể chạy lại đầy đủ với toàn bộ data cần thiết.

## Ưu điểm

* Native AWS, không cần cài thêm agent hay sidecar vào cluster.
* Có Vault Lock và encryption at-rest, phù hợp với các yêu cầu compliance như HIPAA hoặc PCI-DSS.
* Pre-provisioned DR cluster giúp RTO thấp hơn đáng kể.
* Quản lý tập trung scheduling, retention và monitoring trong một service.

## Nhược điểm

* Phải trả tiền duy trì DR cluster liên tục dù không dùng thường xuyên.
* Phần restore metadata, ví dụ EBS snapshot ARN mapping, khá dễ lỗi nếu chưa automation.
* Không tự động failover, vẫn cần can thiệp thủ công khi có sự cố.
* Cross-account restore cần cấu hình IAM thêm.

## AWS Backup hay Velero?

Cả hai đều giải quyết được bài toán backup và restore EKS, nhưng hướng tiếp cận khác nhau.

AWS Backup là managed service, setup ít bước hơn, có cross-region copy native, Vault Lock và audit log phù hợp cho compliance. Điểm trừ là chỉ phù hợp khi chạy trên AWS và sẽ tốn phí theo lượng backup.

Velero thì linh hoạt hơn, có thể chạy multi-cloud, filter theo namespace hoặc exclude resource type. Đổi lại team phải tự lo storage backend, tự cấu hình replication và tự vận hành.

Nên dùng AWS Backup khi:

* Toàn bộ stack nằm trên AWS.
* Team cần compliance audit trail.
* Muốn giảm operational overhead.
* Đã quen với AWS Console hoặc AWS CLI.

Nên dùng Velero khi:

* Hệ thống chạy multi-cloud hoặc hybrid.
* Cần flexibility cao như filter namespace, exclude resource type.
* Muốn kiểm soát hoàn toàn backup storage.
* Budget được ưu tiên hơn sự tiện lợi.

## Kết

AWS Backup cho EKS là một bước tiến thực tế trong việc làm DR dễ tiếp cận hơn cho các team vận hành Kubernetes trên AWS. Composite recovery point, cross-region copy và vault security đều nằm trong một managed service, nên team không cần tự dựng pipeline riêng từ đầu.

Tuy vậy, đây vẫn là warm standby DR chứ không phải một nút bấm thần kỳ. RTO thực tế phụ thuộc vào việc DR cluster đã được chuẩn bị sẵn chưa, metadata có đúng không và team có drill thường xuyên không.

DR tốt là DR được test thường xuyên, không phải DR chỉ đẹp trên paper.

**Nguồn tham khảo:** [Cross-Region disaster recovery for Amazon EKS using AWS Backup](https://aws.amazon.com/vi/blogs/containers/cross-region-disaster-recovery-for-amazon-eks-using-aws-backup/)
