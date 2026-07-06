---
title: "Deploy AWS Organizations with CloudFormation"
date: 2026-04-17
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# [AWS Organizations] Deploy AWS Organizations with CloudFormation

![AWS Organizations with CloudFormation](/workshop_phamquyetchien/images/blog/blog2.png)

AWS has officially added CloudFormation support for AWS Organizations, making it possible to manage an organization structure using Infrastructure as Code (IaC). This is an important update for companies that operate multi-account AWS environments.

Previously, creating Organizational Units (OUs), AWS accounts, and governance policies in AWS Organizations was often done manually through the AWS Console or AWS CLI. As the number of accounts grows, this process becomes more complex, time-consuming, and harder to keep consistent across environments.

With CloudFormation, users can define and deploy AWS Organizations resources through templates. This allows teams to create, update, or delete organization structures in a more automated and controlled way.

## Supported resource types

CloudFormation currently supports three main resource types:

* `AWS::Organizations::Account`: creates a new AWS account and automatically adds it to the Organization.
* `AWS::Organizations::OrganizationalUnit`: creates an OU under the Root or under a parent OU.
* `AWS::Organizations::Policy`: creates and manages policies such as Service Control Policies (SCPs), Tag Policies, Backup Policies, and AI Services Opt-Out Policies.

## Deployment example

In the example shared by AWS, a template can automatically create OUs such as Infrastructure, Production, and Security. The same template can also create AccountA and deploy SCPs to prevent accounts from leaving the Organization or to prevent CloudTrail from being disabled.

Tag Policies can also be applied to standardize tagging across the AWS environment. This helps teams manage resources more easily, especially when the number of accounts and workloads increases.

## Benefits

The biggest benefit of this feature is that organizations can manage AWS Organizations as code. The organization structure can be stored in Git, reviewed before changes, and integrated into automation workflows.

As a result, teams can scale or modify the existing structure more quickly, deploy new environments more consistently, and apply governance policies across multiple AWS accounts.

## Conclusion

CloudFormation support for AWS Organizations is an important step toward standardizing AWS governance at scale. It reduces manual work, improves consistency, and supports infrastructure management through Infrastructure as Code.

For multi-account environments, this is a useful direction because it makes AWS account governance clearer and easier to control.

**Reference:** [Deploy AWS Organizations resources by using CloudFormation](https://aws.amazon.com/vi/blogs/security/deploy-aws-organizations-resources-by-using-cloudformation/)
