---
title: "Event 3"
date: 2026-08-08
weight: 5
chapter: false
pre: " <b> 4.5. </b> "
---
# Event Report: AWS FCAJ Agent Forge – Deepdive (Day 2)

### Event Overview

AWS FCAJ Agent Forge – Deepdive (Day 2) was the second session in an advanced workshop series organized with the First Cloud AI Journey (FCAJ) community. Built from practical AWS engineering materials and experience, the L300 session focused on turning a basic AI Agent into an enterprise-ready Agentic AI system.

The workshop covered memory management, response evaluation, observability, and performance optimization.

### Speakers and Instructor

- **Hiếu:** Co-head of the FCAJ community and Solution Architect at AWS Vietnam.
- **Hải Anh:** Cloud Consultant at Chiase Pacific and the hands-on lab instructor.

### Event Objectives

- Understand long-running conversation state through Short-term Memory and Long-term Memory.
- Learn the OpenTelemetry-based observability model: Logs, Traces, and Metrics.
- Explore automated evaluation to assess response quality and reduce hallucinations and reasoning errors.
- Recognize the security, cost, and operational requirements for production Agents.

### Workshop

The Agent Forge Deepdive series runs across three days, progressing from fundamentals to production practices with Amazon Bedrock AgentCore.

1. **Day 1 (August 1): AgentCore Foundations** — Runtime, Gateway, Identity, and core architecture.
2. **Day 2 (August 8): Memory, Evaluations, Observability & Optimization** — memory management, Agent evaluation, monitoring, and performance tuning.
3. **Day 3 (August 15): DevOps, Policies & Production Best Practices** — DevOps, policies, security, and production practices.

### Key Highlights

#### Agent memory

Agent Memory helps an Agent work beyond the Context Window limit, retain conversational context, and personalize the user experience.

**Short-term Memory** synchronously stores raw chat history so the Agent can maintain a coherent current conversation. It also supports branching, similar to creating branches in Git.

**Long-term Memory** works asynchronously. It extracts important information from conversations and stores it as vectors for later retrieval. The four core strategies are:

- **Summary:** compresses conversation content.
- **User Preference:** stores user preferences.
- **Semantic:** stores domain knowledge.
- **Episodic:** records decisions and past events.

**Namespaces** act as a hierarchical structure that isolates memory by strategy, actor, or session. Semantic search and similarity ranking help retrieve relevant information, reduce token use, and improve response time.

#### Observability

The workshop highlighted the principle: *“You cannot fix what you cannot see.”* The OpenTelemetry-based observability model collects three core types of data:

- **Logs:** detailed request, connection-error, system-error, and terminal records.
- **Traces:** the complete journey of a request, from a user prompt to the Agent response, including tool calls.
- **Metrics:** quantitative measures such as token cost, error rate, and latency.

This information helps teams identify delays, control token costs, and improve user experience.

#### Agent evaluation

AI Agents can hallucinate: present incorrect information as fact. To reduce this risk, the platform provides 13 built-in evaluators, including *correctness* and *helpfulness*.

Evaluation can be performed at three levels:

- **Session level:** evaluates the overall session outcome.
- **Trace level:** evaluates the correctness of a response.
- **Span level:** evaluates individual processing steps, such as tool calls and parameter passing.

**On-demand** evaluation is useful during development and testing, while **Online** evaluation monitors Agent quality in real time in production. Automated results should still be reviewed by subject-matter experts.

### What I Learned

- I understand the synchronous and asynchronous differences between Short-term and Long-term Memory.
- I learned the Logs, Traces, and Metrics pillars of observability and the role of OpenTelemetry in understanding system health.
- I understand how standardized evaluators can assess Agent responses more consistently.
- I learned about Cedar Policy and sandboxing, which support safe Agent actions and code experimentation.

### Key Takeaways

The most memorable idea for me was the **“6/94”** mindset. In a real Agentic AI system, the AI model is only a small part of the solution. Most work belongs to software engineering: memory, identity, gateway security, observability, evaluation, and optimization.

Healthy infrastructure does not automatically mean users are satisfied. Even if servers have no incidents, an Agent can still respond slowly or produce unsuitable results. Latency and response quality must therefore be monitored across infrastructure, application, and model layers.

Finally, Agents should follow the **least-privilege principle**. Cedar policies and sandboxing restrict access and help prevent unwanted actions or effects on system resources.

### Workshop Experience

This session gave me a more practical view of the requirements for operating an enterprise AI Agent. Beyond the language model, a complete system needs memory, monitoring, quality evaluation, and strong security controls.

The Day 2 material provides an important foundation for learning how to build safe, optimized, and scalable Agents.

## Event Photos

<div style="display: flex; gap: 12px; justify-content: space-between; align-items: center; margin: 15px 0;">
	{{< event-image src="/images/4-EventParticipated/event3-1.jpg" alt="Event 3 photo 1" >}}
	{{< event-image src="/images/4-EventParticipated/event3-2.jpg" alt="Event 3 photo 2" >}}
	{{< event-image src="/images/4-EventParticipated/event3-3.jpg" alt="Event 3 photo 3" >}}
</div>
