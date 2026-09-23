# AI Security Lab

My working documentation hub for realistic AI security projects — from research and framework mapping to hands-on tooling and red team work.

This repo is where I track what I'm building, what I've shipped, and what I'm learning. Every project here is grounded in a publicly documented incident, a known framework, or a real attack class — not hypotheticals.

**Status:** Active | **Last updated:** 2026-09-23

---

## What This Is

A public record of my work in AI security, organized so that:

- **Non-technical work counts.** Incident analyses, framework crosswalks, and governance templates are first-class artifacts here, not filler.
- **Every project has evidence.** Each entry traces back to a source — an incident, a paper, a disclosure, a framework.
- **Progress is visible.** Projects move through `Planned → In Progress → Draft → Published`.
- **Nothing is padded.** If something is inferred rather than proven, it says so.

The goal is a body of work I can point to — for employers, collaborators, or anyone trying to get into AI security without a clear on-ramp.

---

## Focus Areas

| Area | Why |
|------|-----|
| **Agent & MCP security** | The same design pattern (broad tool access via MCP) sits behind most recent AI incidents. |
| **Prompt injection & tool-output trust** | Indirect injection through tool output is the attack class traditional controls miss. |
| **Excessive agency & least privilege** | The amplifier risk — turns a model failure into a real-world action. |
| **AI governance & shadow AI** | The condition that makes every other risk worse. |
| **Framework mapping** | OWASP LLM Top 10, NIST AI RMF, MITRE ATLAS — the common language employers expect. |

---

## Project Tracker

Status legend: ✅ Published · 🟡 In Progress · 🔵 Draft · ⚪ Planned

### Tier 1 — Research & Writing

| # | Project | Status | Artifact |
|---|---------|--------|----------|
| 1 | AI Incident Register — standardized incident write-ups | 🟡 | [Nimbus DevSecOps Risk Register](./incident-register/nimbus-devsecops.md) |
| 2 | Framework Crosswalks — OWASP LLM ↔ NIST ↔ ATLAS | ⚪ | — |
| 3 | Governance Templates — AI AUP, MCP approval checklist | ⚪ | — |
| 4 | Threat Landscape Digest — monthly research summary | ⚪ | — |
| 5 | Tabletop Scenarios — IR exercises for AI systems | ⚪ | — |

### Tier 2 — Hands-On, Low Barrier

| # | Project | Status | Artifact |
|---|---------|--------|----------|
| 6 | Prompt Injection Corpus — categorized payloads | ⚪ | — |
| 7 | MCP Server Inventory — public servers by tool surface | ⚪ | — |
| 8 | Agent Permission Matrix — tool-by-tool analysis | ⚪ | — |
| 9 | Red Team Write-Ups — attacks on safe targets | ⚪ | — |
| 10 | Telemetry Templates — agent behavioral logging | ⚪ | — |

### Tier 3 — Build, Test, Break

| # | Project | Status | Artifact |
|---|---------|--------|----------|
| 11 | MCP Security Scanner | ⚪ | — |
| 12 | Prompt Injection Detection Harness | ⚪ | — |
| 13 | Agent Behavior Monitor | ⚪ | — |
| 14 | Tool-Output Sanitizer | ⚪ | — |
| 15 | Incident Reproduction Labs (Dockerized) | ⚪ | — |
| 16 | AI Security CTF Challenges | ⚪ | — |

---

## Featured Work

### Nimbus DevSecOps AI Risk Register

A worked example of an AI risk register for a fictional DevSecOps company running three AI systems (coding agent, support chatbot, incident-response agent). Eight risks, each grounded in a real, publicly documented incident and mapped to OWASP LLM Top 10 and NIST AI RMF.

- **What it demonstrates:** incident-to-framework mapping, risk prioritization, mitigation design, and clear separation of documented vs. derived risks.
- **Key finding:** excessive agent permissions (R-08) amplify every other risk — the design-level issue behind most AI incidents.
- **Read it:** [incident-register/nimbus-devsecops.md](./incident-register/nimbus-devsecops.md)

---

## Repository Structure
