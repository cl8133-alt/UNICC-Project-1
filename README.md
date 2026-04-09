# UNICC AI Safety Lab
## Project 1 — Research and Platform Preparation

**Student:** Coreece Lopez — Project 1 Manager  
**Course:** NYU MASY GC-4100 Applied Project Capstone — Spring 2026  
**Sponsor:** Dr. Andres Fortino | Client: UNICC | Liaison: Joseph Papa  
**Team:** Coreece Lopez (P1) · Feruza Jubaeva (P2) · Galaxy Okoro (P3)  
**GitHub:** [https://github.com/cl8133-alt/unicc-ai-safety-lab-p1](https://github.com/cl8133-alt/unicc-ai-safety-lab-p1)  
**Date:** April 2026

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Research Question](#research-question)
3. [Project Sequence](#project-sequence)
4. [What This Project Delivers](#what-this-project-delivers)
5. [System Architecture Summary](#system-architecture-summary)
6. [Governance Framework Alignment](#governance-framework-alignment)
7. [SLM Platform Validation](#slm-platform-validation)
8. [Fall 2025 Solution Analysis](#fall-2025-solution-analysis)
9. [Prerequisites and Setup](#prerequisites-and-setup)
10. [How to Run the Baseline Test](#how-to-run-the-baseline-test)
11. [Repository Structure](#repository-structure)
12. [Deliverable Traceability](#deliverable-traceability)
13. [Documentation Index](#documentation-index)

---

## Project Overview

Project 1 establishes the **foundational research, system architecture, governance framework mapping, and validated model environment** for the UNICC AI Safety Lab. This project provides the theoretical underpinnings, technical specifications, and operational infrastructure upon which Projects 2 and 3 build the functional AI safety evaluation system.

The UNICC AI Safety Lab is a **multi-module inference ensemble** (council of experts) that evaluates AI agents for trust, safety, and governance compliance before production deployment within the UNICC AI Sandbox. It is designed for the **United Nations International Computing Centre (UNICC)**, the UN's largest digital solutions and cybersecurity partner, to ensure AI systems deployed across UN agencies meet the highest standards of safety, ethics, and governance.

This project was developed in response to the Spring 2026 UNICC Capstone Memorandum, which directed NYU SPS students to transform the **top three Fall 2025 AI safety testing solutions** into a unified, auditable, standalone evaluation environment operating on open-source small language models (SLMs) deployed on the NYU DGX Spark cluster.

---

## Research Question

> *"How can a standalone Small Language Model, trained on open-source LLM weights and deployed in an on-premises environment, be systematically evaluated, stress-tested, and incrementally trained to support governance, risk, and compliance (GRC) testing of AI bots and agents — especially for trust, safety, and security assurance in institutional settings?"*

— UNICC AI Safety Lab Memorandum, Spring 2026

---

## Project Sequence

Project 1 is completed first and provides the foundation for everything that follows.

```
Project 1 (Coreece)  →  Project 2 (Feruza)  →  Project 3 (Galaxy)
Research & Platform      Council of Experts       Testing, UX &
Preparation              Engine Development        Integration
```

| Phase | Owner | Deliverables |
|-------|-------|-------------|
| **Project 1** | Coreece Lopez | Architecture blueprint, JSON schema, governance mapping, integration spec, validated SLM platform |
| **Project 2** | Feruza Jubaeva | Three independent judge modules, orchestration/arbitration logic, Llama Guard integration, CLI/API pipeline |
| **Project 3** | Galaxy Okoro | Web interface, comprehensive test suite, visualization tools, end-user documentation, usability testing |

---

## What This Project Delivers

Project 1 does **not** write judge code, council orchestration code, or interface code — those belong to Projects 2 and 3. Project 1 delivers:

| # | Deliverable | Location | Status |
|---|------------|----------|--------|
| 1 | **Functional Requirements Specification (FRS)** | `docs/FRS.md` | Complete |
| 2 | **System Architecture Blueprint** | `docs/architecture.md` | Complete |
| 3 | **Governance Framework Mapping** | `docs/governance_mapping.md` | Complete |
| 4 | **Integration Specification** | `docs/integration_spec.md` | Complete |
| 5 | **Deployment Configuration** | `docs/deployment_config.md` | Complete |
| 6 | **Unified JSON Output Schema** | `schemas/output_schema.json` | Complete |
| 7 | **Baseline Model Validation Script** | `scripts/test_model.py` | Complete — 20/20 prompts passed |
| 8 | **Baseline Test Logs** | `logs/sample_test_log.jsonl` | Complete — real non-zero scores confirmed |
| 9 | **Literature Survey** | `docs/literature_survey.md` | Complete |
| 10 | **Situational Analysis** | `docs/situational_analysis.md` | Complete |

---

## System Architecture Summary

The AI Safety Lab follows a **Council of Experts** architecture — three independent expert judges backed by Meta Llama Guard 3 safety classification, orchestrated through conservative (strictest-wins) arbitration.

```
                    ┌──────────────────┐
                    │   Input Layer    │
                    │ GitHub / Text /  │
                    │   Web Interface  │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │   Test Generation │
                    │ 25+ Adversarial  │
                    │     Prompts      │
                    └────────┬─────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
     ┌────────▼───┐  ┌──────▼─────┐  ┌─────▼──────┐
     │  Security   │  │  Ethics    │  │ Governance  │
     │   Judge     │  │  Judge     │  │   Judge     │
     │ S1,S2,S7,   │  │ S3,S4,S6,  │  │ S5,S8,S13  │
     │ S9,S14      │  │ S10,S11,S12│  │             │
     └──────┬──────┘  └─────┬──────┘  └──────┬──────┘
            │               │                │
            │       ┌───────▼────────┐       │
            └──────►│  Llama Guard 3  │◄─────┘
                    │  Text + Vision  │
                    └───────┬────────┘
                            │
                   ┌────────▼─────────┐
                   │   Orchestrator    │
                   │  & Arbitration    │
                   │ (Strictest-Wins)  │
                   └────────┬─────────┘
                            │
                   ┌────────▼─────────┐
                   │    Reporting &    │
                   │   Web Interface   │
                   │  JSON/CSV/HTML    │
                   └──────────────────┘
```

### Core Design Principles

1. **Independent Expert Perspectives** — Each judge evaluates from a distinct lens (security, ethics, governance) to prevent single-point-of-failure assessment
2. **Conservative Arbitration** — The strictest verdict wins (precautionary principle), appropriate for high-stakes UN/humanitarian contexts
3. **Llama Guard 3 as Primary Safety Signal** — Meta's MLCommons-aligned taxonomy (S1–S14) provides standardized, reproducible safety classification
4. **Disagreement Surfacing** — When judges disagree, the system flags the disagreement and escalates to human oversight rather than silently averaging
5. **Full Audit Trail** — Every evaluation produces timestamped, structured evidence (scores, rationale, flags) for reproducibility and accountability

### Technology Stack

| Component | Technology | Rationale |
|-----------|-----------|-----------|
| Safety classification | Meta Llama Guard 3 (8B text, 11B vision) | Open-weight, MLCommons-aligned, 14-category taxonomy |
| Reasoning model | Mistral 7B Instruct v0.3 | Open-weight, auditable, no API key required |
| Model serving | Ollama (local) | On-premises deployment, no internet dependency |
| Evaluation pipeline | Python 3.11+ | Portable, no external framework dependency |
| Web interface | Flask + vanilla JS | Lightweight, single-page application |
| Deployment target | NYU DGX Spark (2-node NVIDIA cluster) | 128 GB GPU memory, deterministic compute |

---

## Governance Framework Alignment

The system maps to **six international governance frameworks**, with OWASP and MITRE ATT&CK being mandatory per the memorandum. Full mapping with implementation details in `docs/governance_mapping.md`.

| Framework | Status | Implementation Depth |
|-----------|--------|---------------------|
| **OWASP Top 10 for LLM Applications (2025)** | Mandatory — Complete | All 10 risks mapped to specific judges and detection logic |
| **MITRE ATT&CK Framework** | Mandatory — Complete | 7 techniques mapped to adversarial test categories |
| **EU AI Act (2024)** | Complete | Articles 5, 9–15, 50 mapped to judge evaluation criteria |
| **NIST AI Risk Management Framework (2023)** | Complete | All 4 functions (Govern, Map, Measure, Manage) implemented |
| **ISO/IEC 42001 (2023)** | Complete | AI management system alignment documented |
| **UNESCO AI Ethics Recommendations** | Complete | 5 principles mapped to system capabilities |

---

## SLM Platform Validation

The baseline validation confirms the standalone SLM environment is operational and ready for Project 2 development.

| Metric | Result |
|--------|--------|
| Total test prompts | 20 (10 safe + 10 adversarial) |
| Successful responses | 20/20 (100%) |
| Pass threshold | 95% (19/20) |
| Primary backend | Ollama — Mistral 7B Instruct |
| Fallback backend | Anthropic API (Claude) |
| Mean response time | 3.4 seconds |
| All scores non-zero | Confirmed |
| Log file | `logs/sample_test_log.jsonl` |

---

## Fall 2025 Solution Analysis

Project 1 analyzed the top three Fall 2025 competition solutions to determine how they should be adapted into the unified council-of-experts architecture:

| Fall 2025 Team | Approach | Adapted As | Key Capabilities Carried Forward |
|---------------|----------|-----------|----------------------------------|
| **Team 1** (Jitong Yu, Haoyu Guo, Ning Sun) | Adaptive adversarial testing with 6-dimension scoring, semantic trigger detection, multi-turn evaluation | **Security Judge** | Prompt injection detection, code safety analysis, PII regex scanning, deception detection, 6 weighted scoring dimensions |
| **Team 6** (Yiwen Mao, Jiakai Niu, Lisa Yu) | Policy compliance platform with governance evaluation | **Governance Judge** | Application sensitivity assessment, capability risk evaluation, monitoring/transparency readiness, deployment readiness scoring, human oversight triggers |
| **Team 8** (Hongyi Yang, Sicong Ma, Meiran Guo) | Ethics evaluation agent with fairness/dignity focus | **Ethics Judge** | Hate/hostility detection, discrimination analysis, vulnerable group targeting, dehumanizing language detection, analytical context differentiation |

### Design Decisions from Analysis

1. **Independent judge architecture** over monolithic evaluator — preserves distinct perspectives per memorandum requirements
2. **Llama Guard 3 as shared safety backbone** — provides standardized S1–S14 classification across all judges while each judge adds domain-specific analysis
3. **Conservative (strictest-wins) arbitration** over majority-vote — appropriate for high-stakes humanitarian/institutional contexts where false negatives carry greater risk than false positives
4. **Multi-dimensional weighted scoring** per judge — adapted from Team 1's 6-dimension approach, extended to all three judges for nuanced risk assessment
5. **Disagreement surfacing** — when judges disagree, the system explicitly flags the disagreement rather than silently resolving it, supporting the council-of-experts transparency principle

---

## Prerequisites and Setup

### Option 1 — Local (No API Key Required)

```bash
# Install Ollama
# Download from https://ollama.com

# Pull required models
ollama pull mistral:7b-instruct
ollama pull llama-guard3:8b
ollama pull llama-guard3:11b-vision

# Start Ollama server
ollama serve

# Run baseline test
python3 scripts/test_model.py
```

### Option 2 — API Fallback (For Sandbox Environments)

```bash
# Create .env file (never push to GitHub)
echo "ANTHROPIC_API_KEY=your_actual_key_here" > .env

# Run baseline test
pip install -r requirements.txt
python3 scripts/test_model.py
```

### DGX Spark Sandbox Submission

1. Submit GitHub repository URL to DGX Spark portal
2. Runtime: Python 3.11
3. Setup command: `pip install -r requirements.txt`
4. Run command: `python3 scripts/test_model.py`

---

## How to Run the Baseline Test

```bash
python3 scripts/test_model.py
```

This runs 20 prompts (10 safe knowledge questions + 10 adversarial probes) and logs every result to `logs/sample_test_log.jsonl`. Real non-zero scores confirm the model is operational. The script tries Ollama first and falls back to the Anthropic API when Ollama is unavailable.

**Pass criteria:** 19 of 20 prompts receive a response (95% success rate).

---

## Repository Structure

```
unicc-ai-safety-lab-p1/
├── README.md                          # This file — project overview
├── requirements.txt                   # Python dependencies
├── .gitignore                         # Excludes .env, __pycache__, logs
│
├── docs/
│   ├── FRS.md                         # Functional Requirements Specification
│   ├── architecture.md                # Council-of-Experts architecture blueprint
│   ├── governance_mapping.md          # OWASP, MITRE, EU AI Act, NIST, ISO, UNESCO mapping
│   ├── integration_spec.md            # Project handoff boundaries and contracts
│   ├── deployment_config.md           # DGX Spark deployment and validation record
│   ├── literature_survey.md           # Academic literature review
│   └── situational_analysis.md        # Industry and stakeholder analysis
│
├── schemas/
│   └── output_schema.json             # Unified judge output contract (P2 ↔ P3)
│
├── scripts/
│   └── test_model.py                  # 20-prompt baseline validation script
│
└── logs/
    └── sample_test_log.jsonl          # Baseline test results (20 prompts, real scores)
```

---

## Deliverable Traceability

Every Project 1 deliverable traces back to the memorandum, project charter, and FRS:

| Memorandum Requirement | Deliverable | Evidence |
|----------------------|------------|---------|
| Review multi-module inference ensembles research | `docs/literature_survey.md` | 30+ academic sources analyzed |
| Analyze Fall 2025 solutions | `docs/architecture.md` §Fall 2025 Analysis | Teams 1, 6, 8 mapped to Security, Ethics, Governance judges |
| Set up SLM environment on DGX Spark | `docs/deployment_config.md`, `logs/sample_test_log.jsonl` | 20/20 prompts passed, real non-zero scores |
| Create documentation | All `docs/*.md` files | 7 specification documents |
| Develop conceptual framework for module interaction | `docs/architecture.md`, `schemas/output_schema.json` | 5-layer architecture + unified JSON contract |
| OWASP alignment (mandatory) | `docs/governance_mapping.md` | All 10 LLM risks mapped to judges |
| MITRE ATT&CK alignment (mandatory) | `docs/governance_mapping.md` | 7 techniques mapped to test categories |
| Detailed technical specification | `docs/FRS.md`, `docs/integration_spec.md` | 12 functional + 5 non-functional requirements |
| Fully operational SLM platform | `scripts/test_model.py` + logs | 100% success rate on DGX Spark |

---

## Documentation Index

| Document | Purpose | Audience |
|----------|---------|----------|
| [FRS](docs/FRS.md) | Functional and non-functional requirements specification | Project team, sponsor, UNICC |
| [Architecture](docs/architecture.md) | System design blueprint with module decomposition | Project 2 developer, technical reviewers |
| [Governance Mapping](docs/governance_mapping.md) | Framework-to-implementation traceability matrix | Governance stakeholders, UNICC compliance |
| [Integration Spec](docs/integration_spec.md) | Handoff contracts between Projects 1→2→3 | All team members |
| [Deployment Config](docs/deployment_config.md) | DGX Spark deployment record and validation | Infrastructure, evaluators |
| [Literature Survey](docs/literature_survey.md) | Academic research underpinning design decisions | Academic reviewers, sponsor |
| [Situational Analysis](docs/situational_analysis.md) | Industry context and strategic justification | Sponsor, UNICC stakeholders |
| [Output Schema](schemas/output_schema.json) | Unified JSON contract for all judge modules | Projects 2 and 3 developers |

---

## Contact

**Coreece Lopez** — cl8133@nyu.edu  
Any architecture or schema change must be communicated to all three team members before implementation.

---

*Project 1 — UNICC AI Safety Lab — NYU MASY GC-4100 — Spring 2026*
