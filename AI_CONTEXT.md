# ATLAS — Global Context (Appliance)

## What is ATLAS?
ATLAS is a self-hosted AI-driven resilience and backup posture assessment platform.
It analyzes backup, disaster recovery, and data protection setups across vendors
and provides explainable scoring, compliance insights, and remediation guidance.

## Role of this Repository
This repository contains the **ATLAS Appliance**, which is the customer-facing,
self-hosted distribution of the product.

It focuses on:
- Deployment
- Installation
- Upgrade
- Runtime wiring
- User-facing documentation

It does NOT contain business logic or intelligence.

## Target Users
- Enterprises
- MSPs / MSSPs
- Backup & infrastructure teams
- Security & compliance teams

## Product Principles
- Self-hosted by default
- Minimal operational complexity
- No vendor lock-in
- Secure by default
- Low support burden

## Architecture Position
atlas-appliance is the outer layer of ATLAS:
- It runs atlas-core as a service
- It embeds n8n for automation
- It integrates with Traefik for ingress
- It exposes no logic itself

## What AI Should Do Here
AI tools (Copilot) may:
- Generate infrastructure code (Docker, Bash)
- Generate documentation
- Generate installation and diagnostic scripts

AI tools must NOT:
- Decide architecture
- Design security or licensing models
- Implement business logic

## Source of Truth
For this repository:
- `docs/AI_BRIEF.md` is the source of truth
- EPICS guide what must exist and why
