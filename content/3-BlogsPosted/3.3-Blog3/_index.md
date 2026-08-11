---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---
# Foundational Knowledge – A Strong Launchpad for Cloud Computing

Hello everyone!

I am interested in backend development and cloud computing. I believe learning technology is not only about using tools; it is more important to understand the core principles behind them.

While learning and practicing AWS, I found that my cloud journey became much easier than I first expected. This was not because AWS is simple or because I could memorize hundreds of services. It was because I had already learned foundational topics such as system design, networking, APIs, databases, and system-building principles.

This knowledge became a map that helped me connect AWS services to real problems instead of learning them as separate concepts.

In this post, I want to share a personal view: foundational knowledge is a strong launchpad that helps us learn cloud technology faster, understand it more deeply, and use it more effectively.

I hope this post gives you a useful perspective if you are starting your cloud-computing journey.

In recent years, cloud computing has become an important skill for developers, system engineers, and data engineers. But when I started learning AWS, I felt overwhelmed by hundreds of unfamiliar services: EC2, VPC, IAM, ECS, Lambda, CloudFront, Route 53, and more.

I asked myself: “Do I need to memorize every one of these services?” My answer is no.

What helped me learn AWS faster was not memorizing service names. It was the foundational knowledge I had learned earlier: system design, APIs, networking, databases, operating systems, and system-building principles. Mapping each foundation to a service use case made it much easier to learn and apply AWS in an architecture.

## AWS does not teach completely new concepts

After studying and practicing, I realized that most AWS services implement principles that have existed for a long time.

- AWS did not create the server concept. It provides EC2.
- AWS did not invent computer networking. It provides VPC, route tables, Internet Gateway, NAT Gateway, security groups, and network ACLs to manage networks in the cloud.
- AWS did not create databases. It provides RDS, Aurora, and DynamoDB to run database systems using different models.
- AWS did not invent HTTP or APIs. It provides API Gateway, load balancers, and other services that help APIs run reliably at scale.
- If you understand a traditional system, learning AWS is not learning a completely new technology. It is mapping foundational knowledge to AWS implementations.

## When you understand the principle, each service has meaning

Before learning AWS, I spent time studying system architecture, network protocols, and how APIs work. Because of that, when I see a new service, I no longer ask:

> “What is this service used for?”

Instead, I ask:

> “What problem needs to be solved in a traditional system architecture? Which AWS service solves that problem?”

This small change in thinking made the learning process much more intuitive.

For example:

- If you understand load balancing first, Application Load Balancer is familiar.
- If you understand subnets and routing first, VPC becomes a familiar network model.
- If you understand reverse proxies, CloudFront and API Gateway are easier to understand.
- If you understand Docker, ECS and ECR are simply the next step for managing containers in cloud infrastructure.
- If you understand database replication, Multi-AZ and Read Replicas are no longer difficult terms.

I do not learn each service separately. I learn by connecting it to knowledge I already have.

## Cloud does not replace foundational knowledge

Cloud does not replace foundational knowledge. It is simply a new environment for applying it.

- An engineer who does not understand computer networking will find it hard to configure a VPC correctly.
- A person who has never designed an API will struggle to choose between API Gateway, a load balancer, and Lambda.
- A person who does not understand high availability will struggle to explain why Multi-AZ, Auto Scaling, and load balancers are needed.
- A person who does not understand security principles may grant IAM permissions only to “make it work,” rather than following least privilege.

Cloud makes systems faster to deploy, but it cannot replace technical thinking.

## Learning cloud is learning to solve problems

What interests me most about AWS is not the number of services. It is how every service was created to solve a real problem.

Whenever I learn a service, I begin with two questions:

- How was this problem solved before the service existed?
- What did AWS do to simplify or improve that solution?

When I can answer these questions, I no longer learn by memorizing names or clicking through the management console. I understand why a service exists, when to use it, and when not to use it. That is the difference between learning a tool and learning how to think.

## Foundational knowledge is a long-term investment

Cloud platforms will change. Service names and management consoles may change. Some technologies will eventually be replaced.

But the principles of computer networking, operating systems, databases, APIs, security, and system design will remain useful for many years.

That is why I believe investing in foundational knowledge is the most valuable long-term investment for a software engineer. It not only helps us learn AWS faster, but also helps us approach any other cloud platform with the same mindset.

## Conclusion

If you are starting to learn cloud technology and feel overwhelmed by hundreds of services, my advice is to stop trying to memorize everything. Return to the fundamentals instead.

Understand how a system works. Understand why it needs networking, APIs, databases, load balancing, security, and system architecture.

Once you understand these first building blocks, AWS will no longer feel like a collection of disconnected services. It becomes an ecosystem where every service has a clear place, purpose, and meaning.

{{< event-image src="images/3-Blog/Blog3.jpg" alt="Blog post 3" >}}

## References

1. [What is Cloud Computing? - AWS](https://aws.amazon.com/what-is-cloud-computing/)
2. [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/wellarchitected-framework.html)
3. [Amazon EC2](https://docs.aws.amazon.com/ec2/)
4. [Amazon VPC](https://docs.aws.amazon.com/vpc/)
5. [AWS Identity and Access Management (IAM)](https://docs.aws.amazon.com/iam/)
6. [Amazon API Gateway](https://docs.aws.amazon.com/apigateway/)
7. [Amazon RDS](https://docs.aws.amazon.com/rds/)
8. [Amazon DynamoDB](https://docs.aws.amazon.com/dynamodb/)

## Link Post

[AWS Study Group post](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2233901344041492/)
