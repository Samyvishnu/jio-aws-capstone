# TPM Master Playbook — Jio AWS Hyperscale Streaming Platform

An interactive, single-file HTML dashboard that serves as a complete Technical Program Management (TPM) playbook for building a Jio-scale, production-ready live OTT streaming platform on AWS.

# Jio AWS Hyperscale Streaming Platform
TPM Master Playbook — AWS Live Streaming Architecture

**Live site:** https://Samyvishnu.github.io/jio-aws-capstone

Playbook : https://Samyvishnu.github.io/jio-aws-capstone/jio-aws-capstone-playbook.html
---

## Overview

This playbook guides a 17-engineer cross-functional team through a 12-week, 6-sprint program to deliver a hyperscale live streaming platform capable of:

- **50M+ concurrent sessions** at **76 Tbps** throughput
- **< 4s glass-to-glass latency** using CMAF (fMP4 + HLS)
- **99.99% monthly availability** with graceful degradation to S3 static fallback
- **AI-driven pre-scaling** via AWS SageMaker, 30 minutes before event kick-off
- **35% CDN cost reduction** through intelligent multi-CDN routing
- **< $300 total AWS dev cost** using a simulation-first development approach

---

## Features

The playbook is a fully self-contained HTML file with interactive navigation across 13 sections:

| Section | Description |
|---|---|
| **Dashboard** | Live program health, sprint progress, and key metrics |
| **Charter** | Executive summary, business objectives, SLOs, scope, and ADRs |
| **Teams** | Cross-functional team map and RACI matrix |
| **Calendar** | 12-week Gantt view and ceremonies/milestones calendar |
| **Sprint 1–6** | Detailed sprint playbooks: planning agendas, daily standups, backlog, retros |
| **Ceremonies** | Templates for standups, reviews, retros, and escalations |
| **Budget** | Cost breakdown by team and AWS service category |
| **Velocity** | Sprint velocity chart and burndown chart (Chart.js) |
| **Risk** | 12-item risk register with likelihood, impact, and mitigation |
| **Comms** | Stakeholder communication templates and escalation paths |

---

## Architecture

The platform is built on AWS (`ap-south-1`) using the following services and patterns:

**Infrastructure & Compute**
- Amazon EKS (Kubernetes) for container orchestration
- VPC with 6 subnets across 2 Availability Zones
- NAT Gateway, Internet Gateway, ALB

**Media Pipeline**
- AWS MediaLive + MediaPackage (FFmpeg simulation during development)
- CMAF low-latency encoding

**Delivery & AI**
- Amazon CloudFront (Multi-CDN strategy via Lambda@Edge routing)
- Amazon Route 53 with health checks and DNS failover
- AWS SageMaker for AI traffic prediction and pre-scaling

**Data & Observability**
- Amazon DynamoDB (sessions + content metadata)
- Amazon Kinesis Data Streams (viewer events)
- Amazon ElastiCache Redis (session state)
- Amazon CloudWatch dashboards, alarms, and AWS X-Ray tracing

**Security**
- AWS WAF + Shield Standard
- AWS Secrets Manager
- Least-privilege IAM with OIDC

**CI/CD**
- AWS CodePipeline + CodeBuild + ECR
- < 15-minute deployment cycle with < 5-minute rollback

---

## Sprint Summary

| Sprint | Weeks | AWS Phases | Goal | Story Points |
|---|---|---|---|---|
| S1 | 1–2 | 0, 1, 2 | Foundation | 40 SP |
| S2 | 3–4 | 3, 4 | Compute & Media | 52 SP |
| S3 | 5–6 | 5, 6 | Delivery & Intelligence | 48 SP |
| S4 | 7–8 | 7, 8 | Data & Observability | 44 SP |
| S5 | 9–10 | 9, 10 | Security & Resilience | 42 SP |
| S6 | 11–12 | 11, 12 | CI/CD & Go-Live | 50 SP |
| **Total** | | | | **276 SP** |

---

## Team Structure

| Team | Size | Primary Sprints |
|---|---|---|
| Platform | 4 Engineers | S1, S2 |
| Media | 3 Engineers | S2, S3 |
| Data & AI | 3 Engineers | S3, S4 |
| Security | 2 Engineers | All (lead in S5) |
| DevOps / SRE | 3 Engineers | S4–S6 |
| TPM + Tech Lead | 2 Leads | All |

---

## Key SLOs

| Metric | Target | Critical Threshold |
|---|---|---|
| Stream latency (glass-to-glass) | < 4s | < 6s |
| Availability (monthly) | 99.99% | 99.9% |
| Time-to-first-frame | < 2s | < 4s |
| 5xx error rate | < 0.01% | < 0.1% |
| CDN cache hit rate | > 95% | > 90% |
| AI prediction accuracy | > 85% | > 75% |
| Deployment rollback time | < 5 min | < 15 min |
| AWS dev cost (total) | < $300 | < $500 |

---

## Usage

No build step or server is required. Open the file directly in any modern browser:

```bash
open tpm-master-playbook.html
```

Navigate between sections using the sticky top navigation bar. All charts (velocity and burndown) render automatically via Chart.js. The Gantt timeline is generated dynamically via inline JavaScript.

---

## Dependencies

All dependencies are loaded from CDN — no local installation needed:

- [Tabler Icons](https://tabler.io/icons) — UI iconography
- [Chart.js 4.4.1](https://www.chartjs.org/) — Velocity and burndown charts

---

## Out of Scope

The following are explicitly excluded from this program:

- VOD (Video on Demand) workflows
- DRM (Digital Rights Management) integration
- Multi-region AWS deployment (single region: `ap-south-1`)
- Mobile application development (iOS / Android)
- Billing and payment microservices
- GDPR / data residency compliance
- Real MediaLive channels during development (FFmpeg simulation used)

---

## Architecture Decision Records (ADRs)

| ADR | Decision | Choice | Rationale |
|---|---|---|---|
| ADR-001 | Container orchestration | EKS / Kubernetes | Industry standard; hybrid on-net capability |
| ADR-002 | Video protocol | CMAF (fMP4 + HLS) | Lowest latency; universal device support |
| ADR-003 | Scaling strategy | AI Pre-Scale (SageMaker) | Reactive HPA too slow for IPL-style traffic spikes |
| ADR-004 | CDN strategy | Multi-CDN | Cost + latency via ISP-level routing |
| ADR-005 | Database | DynamoDB | Auto-scales; sub-1ms; serverless |
| ADR-006 | Degradation strategy | S3 Static Fallback | Users see scores page, not a blank screen |

---

## Cost Guardrails

- A **$5 billing alarm** acts as a hard stop during development
- EKS clusters are deleted immediately after each practice session
- Simulation-first: no MediaLive or production EKS until the appropriate AWS phase block
- All sensitive credentials are managed exclusively through AWS Secrets Manager
