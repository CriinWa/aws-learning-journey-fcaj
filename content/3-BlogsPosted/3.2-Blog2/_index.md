---
title: "Blog 2"
date: 2026-07-31
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---
# AMAZON RDS AUTOMATION ON SCHEDULE

Hello everyone, I have been learning more about AWS cost optimization and read the AWS Prescriptive Guidance article Automatically stop and start an Amazon RDS DB instance using AWS Systems Manager Maintenance Windows.

The article explains how to **automatically start and stop Amazon RDS** on a schedule. For example, a database can run only during business hours and stop at night or on weekends. This is useful for development, testing, and staging environments that do not need to run 24/7.

At first, I thought this would require EventBridge, Lambda, and custom code to call the RDS API. However, AWS Systems Manager already provides the `AWS-StartRdsInstance` and `AWS-StopRdsInstance` Automation runbooks, so almost no extra logic is needed.

Key points:

- Create two Maintenance Windows: one for starting and one for stopping.
- Use cron expressions to set the run time.
- Add a tag to the RDS instances that should be included.
- Put the instances in a Resource Group.
- Use Automation runbooks to start or stop the database automatically.

When there are many RDS instances, the same tag can be applied to all of them instead of configuring every database manually. Systems Manager then runs the task for all resources in the group.

What I learned:

- Not every resource needs to run 24/7, especially development and testing environments.
- Before writing Lambda automation, check whether AWS already provides a suitable runbook.
- An IAM role should be allowed to start and stop only the required RDS instances.
- RDS can be stopped for at most seven consecutive days. AWS then starts it again for maintenance.

My project currently keeps the database running continuously and stops it only when it is not in use. This article showed me a simple way to reduce costs without building too many extra components.

{{< event-image src="images/3-Blog/Blog2.png" alt="Blog 2" >}}

## References

[Automatically stop and start an Amazon RDS DB instance using AWS Systems Manager Maintenance Windows](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/automatically-stop-and-start-an-amazon-rds-db-instance-using-aws-systems-manager-maintenance-windows.html)

## Link Post

[AWS Study Group post](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2230171591081134/)
