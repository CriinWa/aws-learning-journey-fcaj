---
title: "Week 4 Worklog"
date: 2026-07-19
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
### Week 4 Objectives:

* Plan the project's Agile sprint.
* Build the basic backend and learn about Docker and CI/CD.
* Deploy the project's infrastructure services across multiple Availability Zones.

### Tasks to Be Completed This Week:

| Day | Task                                                                                                                                                                                              | Start Date | Completion Date | Reference Material                                                                                                                                                                              |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2   | - Create the project architecture diagram.<br />- **Practice:** Configure IAM policies and roles to allow the application to access AWS services.                                           | 13/07/2026 | 13/07/2026      | [Granting authorization to access AWS services](https://000048.awsstudygroup.com/)<br /><br />[Limitation of user rights with IAM Permission](https://000030.awsstudygroup.com/)                  |
| 3   | - Plan the Agile sprint.<br />- **Practice:** Use tags and resource groups to manage resources.                                                                                             | 14/07/2026 | 14/07/2026      | [Manage Resources Using Tags and Resource Groups](https://000027.awsstudygroup.com/)                                                                                                             |
| 4   | - Learn about Docker and CI/CD.<br />- Prepare CloudFormation templates for IAM users, roles, and policies.<br />- **Practice:** Manage account and project security with AWS Security Hub. | 15/07/2026 | 15/07/2026      | [AWS Security Hub](https://000018.awsstudygroup.com/)<br /><br />[Docker Build Documentation](https://docs.docker.com/build/ci)<br /><br />[AWS CloudFormation](https://000037.awsstudygroup.com/) |
| 5   | - Create an RDS MySQL database and design the database schema.<br />- Learn how to deploy the project with Docker and CI/CD.                                                                      | 16/07/2026 | 16/07/2026      | [Docker Build Documentation](https://docs.docker.com/build/ci)                                                                                                                                   |
| 6   | -**Practice:** Encrypt data with AWS KMS, verify encryption, and query data with Amazon Athena.<br />- Finalize the project configuration.                                                  | 17/07/2026 | 17/07/2026      | [Encrypt at rest with AWS KMS](https://000033.awsstudygroup.com/)                                                                                                                                |

### Week 4 Achievements:

- Plan the schedule the team needs to complete the work.

  - **Sprint 1 – Foundation & Infrastructure:** Planned and built the AWS foundation for the project, including a multi-AZ VPC, public/private subnets, an Internet Gateway, a NAT Gateway, and route tables.
  - **Sprint 2 – CI/CD Pipeline & Deployment:** Planned the GitHub Actions pipeline, ECS Fargate, Application Load Balancer, health checks, and Django/React application configuration on AWS.
  - **Sprint 3 – Scaling, Monitoring & Go-Live:** Planned ECS Auto Scaling, CloudFront, CloudWatch, cost alerts, end-to-end testing, and deployment documentation.
  - **Sprint 4 – Testing & Documentation:** Planned testing of the application's main flows, issue fixes, and completion of deployment, architecture, and project-report documentation.

* Resource management and classification:

  * Used tags to label and classify AWS resources, and AWS Resource Groups to group resources by tags for automated bulk management.
  * Configured AWS CloudTrail to record system activity and used Amazon Athena to query and analyze log data.
* Security:

  * Implemented IAM permission boundaries to define the maximum permissions available to users and groups.
  * Used AWS Security Hub to consolidate security findings from services such as GuardDuty, Inspector, and Macie into one dashboard and run automated checks.
  * Practiced protecting data at rest by configuring AWS KMS encryption for S3 data.
* Applications and containerization:

  * Used Docker to create isolated application environments and reduce issues caused by differences between local and server environments.
  * Integrated Docker into the Continuous Integration process for consistent, automated code testing and integration.
