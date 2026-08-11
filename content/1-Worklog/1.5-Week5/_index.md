---
title: "Week 5 Worklog"
date: 2026-07-26
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Complete the frontend, backend, and core API endpoints.
* Configure Dockerfiles, containerize the application, and manage images in Amazon ECR.
* Practice deploying network infrastructure and containerized applications to ECS with an Application Load Balancer.

### Tasks to Be Completed This Week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - **Practice:** Configure Route 53 for hybrid DNS and review the DNS requirements for the system. | 20/07/2026 | 20/07/2026 | [Set up Hybrid DNS with Route 53](https://000010.awsstudygroup.com/) |
| 3 | - **Practice:** Deploy the application with Docker and use EC2, RDS, and ECR services. | 21/07/2026 | 21/07/2026 | [Deploy Application on Docker](https://000015.awsstudygroup.com/) |
| 4 | - Build API endpoints, configure the backend, and learn about ALB configuration. | 22/07/2026 | 22/07/2026 | [Security groups for your Application Load Balancer Documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-update-security-groups.html) |
| 5 | - **Practice:** Deploy application images to ECS.<br />- Build and debug API endpoints, and complete the user interface. | 23/07/2026 | 23/07/2026 | [Deploy applications on Amazon Elastic Container Service](https://000016.awsstudygroup.com/) |
| 6 | - Test Docker and Amazon ECR. | 24/07/2026 | 24/07/2026 | [Amazon Elastic Container Registry Documentation](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html) |

### Week 5 Achievements:

* Networking and DNS:

  * Understood hybrid DNS architecture and how to integrate on-premises DNS resolution with AWS through Amazon Route 53.
  * Practiced configuring Route 53 Resolver, including inbound endpoints, outbound endpoints, and resolver rules for DNS forwarding between environments.
* Project development:

  * Built and completed API endpoints for the backend service.
  * Resolved issues in sending, receiving, and displaying data between the frontend and backend.
  * Reviewed backend configuration, environment variables, and database connectivity in preparation for containerization and deployment.
* Application containerization:

  * Created frontend and backend Dockerfiles to package the source code.
  * Built and ran containers locally, verifying ports, environment variables, and communication between containers.
  * Tested and fixed Dockerfile, dependency, application configuration, and container startup issues.
  * Verified access and authentication between Docker, Amazon ECR, and AWS services.
* Deploying the application on Amazon ECS:

  * Understood the transition from running Docker containers manually on Amazon EC2 to using Amazon ECS as a managed container service.
  * Practiced creating an ECS cluster and task definitions for frontend and backend containers.
  * Learned and practiced configuring ECS services, ALBs, listeners, and target groups to route traffic to the application.
  * Resolved issues involving environment variables, container ports, security groups, IAM permissions, and pulling images from Amazon ECR.
