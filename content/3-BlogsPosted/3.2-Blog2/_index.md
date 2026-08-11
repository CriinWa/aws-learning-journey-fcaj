---
title: "Blog 2"
date: 2026-07-31
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---
# PREVENTING DATA EXFILTRATION ON AWS WITH EGRESS CONTROLS

## 1. Introduction: the dangerous blind spot

When teams design cloud security, they often focus heavily on inbound traffic (ingress) such as WAF, security groups, and permission policies. Outbound traffic (egress), however, is often left open by default to avoid breaking dependent applications.

That is risky. AWS used the React2Shell vulnerability (CVE-2025-55182) as an example: as soon as the vulnerability became public, attackers immediately targeted unpatched servers to gain remote code execution. After the initial compromise, they established command-and-control connections and quietly exfiltrated data. Without egress controls, that traffic could leave unnoticed.

## 2. The agentic AI era: data leakage risk grows exponentially

The AWS blog emphasizes that egress control is not only for traditional applications. It is also critical for AI agents.

According to OWASP for AI applications, two major risks are:

- Agent Goal Hijack (ASI01): Attackers use prompt injection to change the agent's goal and trick it into sending data out secretly.
- Unexpected Code Execution (ASI05): An AI agent is manipulated into generating and running code that opens a reverse shell or sends sensitive data to an external endpoint.

Because AI agents often need to call external APIs to function, they become high-value targets, and their outbound network traffic must be controlled as tightly as any other system.

## 3. AWS hub-and-spoke security architecture

To address this problem, AWS recommends a layered security architecture. The data and controls are distributed as follows:

- VPC layer: Workloads live in spoke VPCs. They prefer AWS service access through VPC endpoints so data does not need to travel over the public Internet. Any traffic that must leave the VPC is routed through Transit Gateway and inspected by Network Firewall.
- IAM layer: Data perimeter SCPs and RCPs create API-level guardrails.
- Detection layer: Services such as GuardDuty, Security Hub, and IAM Access Analyzer continuously scan for unusual activity.
- Integration layer: When an issue is detected, EventBridge triggers Lambda for automated response and sends notifications through SNS.
- Observability layer: All logs are centralized in CloudWatch for monitoring.

## 4. Preventive controls

These controls stop data before it can escape:

- AWS Network Firewall: Provides deep packet inspection from Layer 3 to Layer 7. It can block unauthorized domains, filter by IP and port, use Suricata rules to detect attack patterns, and even decrypt TLS to catch exfiltration hidden inside HTTPS.
- Amazon Route 53 Resolver DNS Firewall: Attackers often use DNS tunneling to hide data inside DNS queries because DNS is usually less scrutinized. DNS Firewall blocks known malicious domains and can detect DGA-style domain generation early with AI and machine learning.
- Data perimeters: Service Control Policies (SCPs) at the organization level prevent loopholes, while Resource Control Policies (RCPs) and VPC endpoint policies ensure only identities in your organization can call data-access APIs.

## 5. Detective controls

If preventive controls fail, the organization needs security cameras to detect the issue:

- Amazon GuardDuty: Detects unusual behavior such as DNS-based exfiltration attempts (Trojan:EC2/DNSDataExfiltration) or API calls from blacklisted IPs (Exfiltration:S3/MaliciousIPCaller).
- IAM Access Analyzer: Uses automated reasoning to continuously check whether any resource, such as an S3 bucket, is publicly exposed or accidentally shared with external accounts.
- AWS Security Hub: Aggregates findings from GuardDuty, IAM Access Analyzer, Inspector, and Macie into one place for a broader view of exfiltration risk.

## 6. Practical rollout plan for enterprises

AWS recommends rolling this out in three stages instead of trying everything at once:

- Stage 1 - Quick wins: Enable Route 53 DNS Firewall and Amazon GuardDuty first to establish a monitoring baseline and address DNS-related risk quickly.
- Stage 2 - Foundational: Implement data perimeters (SCPs and RCPs) and add AWS Network Firewall to the Transit Gateway architecture.
- Stage 3 - Efficient: Enable IAM Access Analyzer, connect EventBridge and Lambda for automated IP/rule blocking, and centralize visibility through Security Hub.

## 7. Lessons learned

This article changed how I think about cloud security. We cannot only lock the front door (ingress); we must also watch the back door (egress) very closely. This becomes even more important when AI models and AI agents are part of the system, because controlling the APIs they can call is essential to prevent prompt injection from leaking company data.

Reference: https://aws.amazon.com/vi/blogs/security/prevent-data-exfiltration-aws-egress-controls-for-cloud-workloads/


![Blog2](https://criinwa.github.io/aws-learning-journey-fcaj/images/3-Blog/Blog2.jpg)
