---
title: "Week 6 Worklog"
date: 2026-08-02
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Deploy the staging environment on ECS Fargate.
* Configure monitoring and test the running system.
* Deploy network infrastructure and containerized applications to ECS with an Application Load Balancer.

### Tasks to Be Completed This Week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Deploy backend and frontend ECS services, Auto Scaling, and CloudWatch. | 27/07/2026 | 27/07/2026 | [Amazon Elastic Container Service Documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-tutorials.html) |
| 3 | - Configure AWS Budgets and SNS. Test project features and the user interface. | 28/07/2026 | 28/07/2026 | [Monolithic app with Docker, ECS and AWS Fargate](https://000067.awsstudygroup.com/) |
| 4 | - Set up CloudWatch Logs and metrics, monitor errors, and perform integration testing. | 29/07/2026 | 31/07/2026 | [CloudWatch Workshop](https://000008.awsstudygroup.com) |
| 5 | - Review logs, health checks, ECS-to-ALB connectivity, and fix issues. | 30/07/2026 | 31/07/2026 | |
| 6 | - Perform functional and integration testing, and document issues that need to be resolved. | 30/07/2026 | 31/07/2026 | |

### Week 6 Achievements:

* Deploying the application on Amazon ECS Fargate:

  * Successfully deployed frontend and backend ECS services with AWS Fargate, including appropriate task definitions, resources, environment variables, and container ports.
  * Integrated ECS services with an ALB and target groups to distribute traffic to application containers.
  * Verified connectivity between the frontend, backend, and supporting system services.
* Scaling and system health:

  * Learned and configured ECS Service Auto Scaling based on CPU and memory utilization.
  * Configured Application Load Balancer health checks to monitor ECS task health.
  * Verified container startup, replacement, and recovery when a task failed or did not pass a health check.
* Monitoring and log management with Amazon CloudWatch:

  * Reviewed logs to identify issues with environment variables, networking, and service ports.
  * Fixed issues and adjusted configuration based on information from ECS, ALB, and CloudWatch.
* Cost management and alerts:

  * Configured AWS Budgets to track resource costs during deployment and operations.
  * Set up Amazon SNS notifications when costs reached configured thresholds.
* System testing:

  * Tested the user interface, functionality, and integration between the frontend and backend.
  * Verified data flows, API responses, application access through the ALB, and container health.
  * Documented issues found during deployment and testing, including configuration, connectivity, health checks, and AWS resource access.
