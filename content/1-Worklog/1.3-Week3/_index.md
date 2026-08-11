---
title: "Week 3 Worklog"
date: 2026-07-12
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Master secure access management and authorization through AWS IAM.
* Understand and deploy relational databases with Amazon RDS, including high availability, backup, and recovery configurations.
* Become familiar with core storage solutions through Amazon S3 and hybrid connectivity through AWS Storage Gateway.
* Select a project topic and define the architecture to be implemented.

### Tasks to Be Completed This Week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Review EC2 knowledge and prepare the project environment. | 06/07/2026 | 06/07/2026 | [Amazon Elastic Compute Cloud Documentation](https://docs.aws.amazon.com/ec2/) |
| 3 | - Learn about S3 and CloudFront.<br />- **Practice:** Configure S3 static website hosting, CORS, and versioning. | 07/07/2026 | 07/07/2026 | [Amazon S3 Documentation](https://docs.aws.amazon.com/AmazonS3/latest/developerguide)<br /><br />[Using File Storage Gateway](https://000024.awsstudygroup.com/)<br /><br />[Starting with Amazon S3](https://000057.awsstudygroup.com/) |
| 4 | - Learn about RDS.<br />- **Practice:** Create and connect to an RDS database.<br />- Meet with the team to select a project topic. | 08/07/2026 | 08/07/2026 | [Amazon RDS and Aurora Documentation](https://docs.aws.amazon.com/rds)<br /><br />[Amazon Relational Database Service (Amazon RDS)](https://000005.awsstudygroup.com/) |
| 5 | - Study IAM, Organizations, Identity Center, KMS, and the principle of least privilege. | 09/07/2026 | 09/07/2026 | [AWS Identity and Access Management](https://000002.awsstudygroup.com/)<br /><br />[IAM Roles and Conditions](https://000044.awsstudygroup.com/) |
| 6 | - Research and consolidate the services to be used, define the project architecture, and create the architecture diagram. | 10/07/2026 | 10/07/2026 | |

### Week 3 Achievements:

* Learned to use Amazon EC2, Amazon S3, AWS Storage Gateway, Amazon VPC, CloudFront, and Amazon RDS.
* Amazon EC2:

  * Learned to select suitable instance types, use AMIs to launch servers quickly, and secure sign-in with key pairs.
  * Distinguished between EBS for persistent network block storage and instance store for high-speed temporary data.
  * Understood how EC2 Auto Scaling adjusts server capacity to match workload and optimize costs.
* Security and identity management:

  * Applied the principle of least privilege with IAM; managed root and IAM users; created restrictive policies; and used IAM roles to grant EC2 instances secure access without exposing access keys.
  * Learned multi-account management with AWS Organizations, single sign-on with IAM Identity Center, and data encryption with AWS KMS.
* Databases:

  * Clarified the differences between relational databases and NoSQL databases.
  * Learned the Multi-AZ and read replica architectures of Amazon RDS and Aurora, along with Redshift for analytics and ElastiCache for caching.
* Amazon S3 and Storage Gateway:

  * Learned the flat object-storage model, S3 Access Points, and storage classes such as Standard, Infrequent Access, and Glacier for cost optimization.
  * Successfully deployed an S3 static website, configured CORS for cross-origin access, and enabled S3 Versioning to protect data.
* Consolidated the planned project architecture and the AWS services to be used, and agreed on the project architecture with the team.
