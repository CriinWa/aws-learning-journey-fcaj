---
title: "Week 7 Worklog"
date: 2026-08-09
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Complete the CI/CD pipeline with GitHub Actions.
* Automate image builds and deployments.
* Test system operation after each update.
* Deploy the system with the CloudFront CDN.

### Tasks to Be Completed This Week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Review Dockerfiles, Docker Compose, Amazon ECR, and the shared application configuration.<br />- Deploy the CloudFront CDN for the application. | 03/08/2026 | 03/08/2026 | [Amazon CloudFront Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html) |
| 3 | - Create a GitHub Actions workflow to check out, build, and validate source code.<br />- Configure AWS authentication, update the ECS task definition, and push images to ECR. | 04/08/2026 | 04/08/2026 | [Configuring OIDC with AWS](https://docs.github.com/en/actions/how-tos/secure-your-work/security-harden-deployments/oidc-in-aws) |
| 4 | - Test the commit → build → deploy workflow.<br />- Monitor ECS rollouts and CloudWatch Logs, and fix issues. | 05/08/2026 | 05/08/2026 | - |
| 5 | - Fix application issues in the data flows. | 06/08/2026 | 06/08/2026 | - |
| 6 | - Update documentation and finalize the team's architecture. | 07/08/2026 | 07/08/2026 | - |

### Week 7 Achievements:

* Deployment configuration:

  * Reviewed and synchronized Dockerfiles, Docker Compose, environment variables, and Amazon ECR configuration.
  * Updated the ECS task definition to support automated deployment.
* CI/CD pipeline:

  * Created a GitHub Actions workflow to automatically validate source code, build container images, and push them to Amazon ECR.
  * Automated ECS task definition updates and deployment of new versions to the ECS service after each source-code update.
* System testing and monitoring:

  * Tested the complete flow: commit, build, image push, and deployment to ECS.
  * Monitored ECS rollouts, CloudWatch Logs, and Application Load Balancer health checks.
  * Identified and resolved issues related to configuration, IAM permissions, containers, and application data flows.
* Documentation completion:

  * Updated deployment and system-testing documentation.
  * Finalized the architecture diagram and added GitHub Actions, Amazon ECR, Amazon ECS, and CloudFront.
