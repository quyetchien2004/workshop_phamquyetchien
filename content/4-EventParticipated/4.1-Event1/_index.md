---
title: "FCAJ Community Day - Conference Call"
date: 2026-05-23
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Event 1: FCAJ Community Day - Conference Call

## Event Information

* **Event name:** FCAJ Community Day - Conference Call
* **Time:** 09:00 - 12:00, Saturday, May 23, 2026
* **Location:** Bitexco Financial Tower, Ho Chi Minh City
* **Role:** Attendee

## Main Content

In this event, I participated as an attendee and took notes from the sessions shared by speakers in the AWS First Cloud AI Journey community. The event focused on AWS, AI, production systems, and how technology can be applied to real-world problems.

The main topics I learned from the sessions were:

### CloudFront as Your Foundation

Nguyen Tuan Thinh's session introduced the role of Amazon CloudFront in delivering content from edge to origin. The session showed that CloudFront is not only for caching content, but also supports security, cost optimization, performance, and resilience.

Key points:

* CloudFront uses a global edge network to reduce latency for users.
* It can be combined with AWS WAF, AWS Shield, Route 53, and S3 to improve security.
* CloudFront can reduce origin load, CPU usage, and Load Balancer cost.
* Features such as Origin Access Control, signed URL, geographic restriction, and origin failover help protect and stabilize the system.

### Context Is Everything

Tinh Truong's session discussed how to use AI more effectively through context. I learned that weak AI responses are not always caused by weak models. In many cases, the user has not provided a clear goal, constraints, or relevant information.

Key points:

* Context helps AI understand the real task.
* More information is not always better. The important thing is providing the right information.
* A good prompt should include a goal, relevant information, constraints, and success criteria.
* Context engineering may become an important skill when working with AI.

### Friendly AI Assistant with Amazon Quick

Hai Anh's session introduced Amazon Quick as an approach for building a friendly AI assistant for business users. The content focused on helping users gather information from multiple sources, automate repeated tasks, and work more efficiently.

Example use cases:

* Automatically creating meeting minutes.
* Sending emails to stakeholders.
* Scheduling the next meeting.
* Connecting data, dashboards, workflows, and AI tools in a unified experience.

### Enterprise-Grade Multi-Agent System

Vy Lam's session explained a multi-agent system for startup credit scoring. I found this topic practical because a single agent may not handle complex problems well when many perspectives are required, such as finance, market, founder team, risk, and compliance.

Key points:

* A multi-agent system can work like a virtual credit committee.
* Each agent focuses on one area, such as financial analyst, market analyst, team evaluator, risk assessor, or compliance.
* Enterprise-grade systems need to consider security, data governance, networking, operations, human factors, and compliance.
* Guardrails are important for controlling input, processing, and output in AI systems.

### Non-Determinism of "Deterministic" LLM Settings

Duc Dao's session helped me understand that even when temperature is set to 0, LLM outputs may still not be perfectly identical across runs. The reasons include GPU floating-point behavior, inference batching, and backend infrastructure optimization.

Key points:

* Temperature 0 removes sampling randomness, but it does not remove every source of variation.
* For high-stakes systems such as healthcare, legal, or finance, the system should be designed to handle output variance.
* Risks can be reduced through multiple runs, majority voting, structured output, and stronger testing.
* When building AI applications, LLM output should not be treated as fully fixed.

### UTMorpho - LotusHacks 2026

Team VIB shared their journey of joining LotusHacks 2026 and building UTMorpho within 36 hours. This session was not focused on one specific AWS service, but it helped me understand the process of turning an idea into a product under time pressure.

Key points:

* Hackathons train the ability to work under time pressure.
* Product building should focus on a real problem instead of trying to include too many ideas.
* Team sync and clear task division are very important.
* Demoing, pitching, and telling the product story are also skills that need practice.

## Lessons Learned

After joining the event, I learned several important points:

* When deploying systems on AWS, performance, security, cost, and resilience should be considered together.
* For AI, context quality has a major impact on output quality.
* Enterprise AI systems need more than working demos. They also need guardrails, logging, security, and control.
* When using LLMs in real products, output variance should be expected and handled by design.
* Joining events helps me learn from real experience shared by the community, not only from documentation or labs.

## Personal Contribution

In this event, I participated as an attendee. I followed the presentations, reviewed the speakers' slides after the event, and took notes about AWS, AI, and system design. These notes helped me improve my internship report and gave me more direction for my personal project in the FCAJ program.

## Participation Evidence

![FCAJ Community Day - Event 1](/images/event/even1.jpg)
