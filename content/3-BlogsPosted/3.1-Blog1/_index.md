---
title: "What if Kubernetes loses an entire Region?"
date: 2026-04-17
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# What if Kubernetes loses an entire Region? AWS Backup can help you recover

![Kubernetes Cross-Region DR with AWS Backup](/workshop_phamquyetchien/images/blog/blog1.png)

Many teams already run EKS with Multi-AZ, but Multi-AZ is not enough if an entire AWS Region has a serious outage. For systems with strict RTO/RPO requirements, such as fintech, ecommerce, or healthcare, a real Cross-Region DR plan is necessary.

In this post, I summarize a practical approach from AWS: using AWS Backup to back up EKS across Regions without building a complicated custom pipeline.

## The problem with stateful workloads on EKS

Backing up stateful applications such as MySQL, Redis, or Kafka on Kubernetes is not as simple as taking a snapshot of an EC2 instance. The backup needs to keep both Kubernetes manifests and persistent volume data consistent at the same point in time, then replicate everything to another Region.

In the past, many teams used Velero for this problem. AWS Backup now also provides native support for EKS. One important difference is that AWS Backup creates a single composite recovery point that includes both Kubernetes resources and EBS snapshots in one backup job. After that, cross-region copy can be done through AWS CLI.

## How it works

1. Deploy EKS in the source Region `us-east-1` with a stateful application, including MySQL 20GB and Redis 10GB on EBS.
2. Create Backup Vaults in both Regions, then run a backup job to create a composite recovery point.
3. Copy the recovery point to the DR Region `us-west-2`.
4. Pre-provision the DR cluster. This is important for reducing RTO because the cluster is already available when restoration is needed.
5. Restore the full application and persistent data into the DR cluster, then verify that MySQL and Redis data match the source.

After a successful restore, the system has 6 pods running, 2 PVCs bound, and an active Load Balancer URL. The application can run again with the required data restored.

## Advantages

* Native AWS service, so there is no need to install extra agents or sidecars into the cluster.
* Vault Lock and encryption at rest help with compliance requirements such as HIPAA or PCI-DSS.
* A pre-provisioned DR cluster can reduce RTO significantly.
* Scheduling, retention, and monitoring are managed centrally in one service.

## Limitations

* The DR cluster still costs money because it must be maintained even when it is not used often.
* Restore metadata, such as EBS snapshot ARN mapping, can be error-prone without automation.
* Failover is not automatic, so manual action is still required during an incident.
* Cross-account restore requires additional IAM configuration.

## AWS Backup or Velero?

Both tools can solve the EKS backup and restore problem, but they take different approaches.

AWS Backup is a managed service. It requires fewer setup steps, supports native cross-region copy, and provides Vault Lock and audit logs that are useful for compliance. The downside is that it is AWS-only and has backup storage costs.

Velero is more flexible. It can work in multi-cloud environments, filter by namespace, and exclude specific resource types. However, the team must manage the storage backend, replication, and operations by themselves.

AWS Backup is a good fit when:

* The whole stack runs on AWS.
* The team needs a compliance audit trail.
* The team wants less operational overhead.
* The team is already familiar with AWS Console or AWS CLI.

Velero is a good fit when:

* The system is multi-cloud or hybrid.
* The team needs more flexibility, such as namespace filtering or excluding resource types.
* The team wants full control over backup storage.
* Budget is prioritized over convenience.

## Conclusion

AWS Backup for EKS is a practical step toward making DR more accessible for teams running Kubernetes on AWS. Composite recovery points, cross-region copy, and vault security are available in a managed service, so the team does not need to build a full backup pipeline from scratch.

However, this is still warm standby DR, not a magic button. The actual RTO depends on whether the DR cluster is ready, whether the metadata is correct, and whether the team practices recovery drills regularly.

Good DR is DR that is tested regularly, not just DR that looks good on paper.

**Reference:** [Cross-Region disaster recovery for Amazon EKS using AWS Backup](https://aws.amazon.com/vi/blogs/containers/cross-region-disaster-recovery-for-amazon-eks-using-aws-backup/)
