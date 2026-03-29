# ☠ GraveShield
### Zombie API Discovery & Defence Platform

> **Hackathon Submission — PS9 | This repository currently contains a frontend demo built for Round 1. The full production system is under active development.**

---

## What is GraveShield?

Most organisations are running more APIs than they can count. New endpoints get spun up for every sprint, every feature flag, every integration — and then they get forgotten. The team that built them moves on, the documentation goes stale, and the API keeps sitting there, quietly exposed to the internet, unmonitored and unpatched.

These are **zombie APIs** — not dead, not alive, just drifting — and they are one of the most overlooked attack surfaces in modern cybersecurity.

GraveShield is an end-to-end platform that **discovers, classifies, risk-scores, and defends** against zombie APIs across an organisation's entire API surface.

---

## Current State — Hackathon Demo (Round 1)

The code in this repository is a **self-contained frontend demo** built for the first round of the hackathon. It is a single HTML file that runs in any browser with zero dependencies, zero setup, and zero backend required.

It is **not** the full system. It is a working demonstration of the core user interface, the scan pipeline flow, and the defence actions that the complete platform will perform.

### What the demo does

- Simulates a full 9-step scan pipeline with realistic step labels: traffic tap → OpenAPI spec parsing → auth enforcement checks → ownership registry cross-reference → call frequency analysis → anomaly detection → CVSS-weighted risk scoring → zombie flagging
- Displays a live endpoint registry of 8 flagged zombie APIs with method, last-seen age, risk score (0–100), and status
- Clicking any row opens a full detail panel showing auth enforcement, owner team, data sensitivity, call volume, schema version, CVE matches, and an analyst note
- Three defence actions per endpoint: **Enable Shadow Proxy**, **Throttle Access**, **Deprecate** — each updates the row state and logs to the live audit feed
- Filter bar to isolate Critical / High / Medium / Shadow endpoints
- Live log feed that streams scan events and defence actions in real time

### How to run it

```bash
# Option 1 — just open it
open index.html

# Option 2 — serve it locally
npx serve .
# or
python -m http.server 8000
```

No npm install. No build step. No environment variables. It opens and works.

### Deploy in 60 seconds

1. Go to [netlify.com/drop](https://app.netlify.com/drop)
2. Drag `index.html` onto the page
3. Share the link

---

## Full System — What We Are Building

The hackathon demo is the UI layer of a much larger system. Below is the complete architecture that will be built out in the phases following this round.

### Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        INPUT LAYER                          │
│   eBPF Traffic Tap  ·  OpenAPI/Swagger  ·  API Gateway Logs │
└────────────────────────────┬────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────┐
│                     DETECTION ENGINE                        │
│        Isolation Forest  ·  DBSCAN  ·  Rule Heuristics      │
│       Call frequency  ·  Auth checks  ·  Schema staleness   │
└────────────────────────────┬────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────┐
│                      RISK SCORING                           │
│         CVSS-weighted pipeline  ·  Data sensitivity         │
│         Ownership registry  ·  CVE matching  ·  Score 0–100 │
└────────────────────────────┬────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────┐
│                      DEFENCE LAYER                          │
│   Envoy Shadow Proxy  ·  Rate Limiting  ·  Auto-Deprecation │
│        Kafka Alerting  ·  Anomaly Notifications             │
└────────────────────────────┬────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────┐
│                       DASHBOARD                             │
│        React  ·  D3.js API Graveyard Map  ·  ELK Audit Log  │
└─────────────────────────────────────────────────────────────┘
```

### Tech Stack

| Layer | Technology |
|---|---|
| Detection Engine | Python, Isolation Forest, DBSCAN, eBPF, OpenAPI Parser |
| Risk Scoring | Python, CVSS-weighted pipeline, custom heuristics |
| Shadow Proxy | Envoy Proxy, NGINX + Lua, traffic mirroring |
| Event Streaming | Apache Kafka, Faust consumers |
| Backend API | FastAPI, PostgreSQL, Redis |
| Frontend | React.js, D3.js, TailwindCSS |
| Deployment | Docker, Kubernetes, Helm |
| Observability | ELK Stack (Elasticsearch, Logstash, Kibana) |

---

## Roadmap

### Phase 1 — Hackathon MVP ✅ (current)
- [x] Single-file deployable dashboard
- [x] Simulated scan pipeline with real step logic
- [x] 8 zombie endpoints with full risk detail panels
- [x] Shadow proxy, throttle, and deprecate actions
- [x] Live audit log feed
- [x] Risk score bar with CVSS-weighted colour coding

### Phase 2 — Beta (Month 1–2)
- [ ] Live eBPF traffic capture agent
- [ ] Real OpenAPI / Swagger spec ingestion
- [ ] FastAPI backend with PostgreSQL endpoint registry
- [ ] Redis-backed real-time state
- [ ] Kafka event streaming pipeline
- [ ] Actual Isolation Forest model on real traffic data
- [ ] Envoy-based shadow proxy with traffic mirroring

### Phase 3 — Production (Month 3+)
- [ ] Enterprise RBAC and SSO integration
- [ ] SIEM integrations — Splunk, Microsoft Sentinel
- [ ] Multi-environment scanning (dev / staging / production)
- [ ] D3.js API graveyard visualisation map
- [ ] Compliance report export — SOC 2, ISO 27001, GDPR
- [ ] Slack / PagerDuty alerting integrations
- [ ] Kubernetes Helm chart for one-command deployment

---

## The Problem We Are Solving

- **40%** of data breaches involve abandoned or stale APIs
- The average enterprise runs **200+ APIs** in production
- Most organisations have **zero monitoring** on endpoints that haven't been called in months
- Zombie APIs bypass modern auth flows, carry unpatched CVEs, and go undetected for weeks after a breach

GraveShield treats this as a discovery and defence problem, not just a governance one.

---

## Project Structure

```
graveshield/
│
├── index.html          # Hackathon demo — full self-contained frontend
├── README.md           # This file
│
└── (coming soon)
    ├── backend/        # FastAPI + PostgreSQL + Redis
    ├── agent/          # eBPF traffic capture agent
    ├── detection/      # ML anomaly detection pipeline
    ├── proxy/          # Envoy shadow proxy config
    ├── streaming/      # Kafka + Faust consumers
    └── infra/          # Docker Compose + Kubernetes Helm charts
```

---

## Hackathon Context

**Problem Statement:** PS9 — "Zombie" (Stale/Defunct) API Discovery and Defence for Cyber Security

This project was built for the hackathon round 1 submission. The demo is intentionally scoped to showcase the core concept, user flow, and defence mechanism clearly within the time constraints. Everything in the roadmap above reflects what the team is actively working toward.

---

## Contributing

This is an active hackathon project. If you are a team member, branch off `main` and open a PR. For everything else, issues and suggestions are welcome once the project moves past the hackathon phase.

---

*GraveShield — Shine a light into the API graveyard.*
