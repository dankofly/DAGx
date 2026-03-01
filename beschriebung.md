# Hypeakz.io — CEO Command Center

**DAGx V1** — Zentrales Dashboard zur Steuerung einer LEAN AI-Agentur auf OpenClaw-Basis.

Steuerung und Ueberwachung von KI-Agenten-Teams (Sales, Dev, SEO/GEO, Law, Accounting) in Echtzeit, von jedem Geraet.

---

## Features

### Operate
- **Dashboard** — Systemzustand auf einen Blick: J-Score, Gateway Health, Live Feed, Agent Roster, Cost Tracker, Resource Monitor
- **Task Board** — Kanban mit 5 Spalten (Recurring → Backlog → In Progress → Review → Done), Drag & Drop, Filtern nach Assignee/Projekt
- **Work Queue** — Echtzeit-Warteschlange orchestrierter Agent-Jobs mit Auto-Refresh
- **Content Pipeline** — Visuelles Kanban fuer Content-Produktion mit konfigurierbaren Stages und Plattform-Tags (YouTube, Blog, X, LinkedIn)
- **Calendar** — Wochenkalender fuer Events, Erinnerungen und Cron-Jobs

### Team
- **Digital Office** — Visuelle Bueroansicht aller Agenten als "Schreibtische" mit Live-Activity-Feed
- **Team Structure** — Organigramm (CEO → Teams → Agenten) mit Detail-Panels, Status-Toggle, Skills-Zuweisung
- **Org Builder** — Erweiterter Organigramm-Editor fuer Abteilungen, Rollen und Hierarchien
- **Skills Registry** — Zentrale Verwaltung aller Skills mit Fitness-Scores
- **Agents** — Agent- und Skill-Verwaltung mit Checkbox-Toggle und DNA-Editor-Link

### Quality
- **Approval Queue** — Genehmigungswarteschlange fuer Agent-Entscheidungen mit Auto-Approve-Regeln und Audit-Trail
- **Council** — Multi-Agent-Beratungsrunden mit Voting und Konsens-Visualisierung
- **Experiments** — A/B-Tests fuer Agent-Konfigurationen und Workflows (Draft → Running → Completed → Analyzed)

### Delivery
- **Blueprints** — Wiederverwendbare Projektvorlagen, instanziierbar in echte Projekte
- **Deliveries** — Pipeline-Tracking (Queued → Working → Review → Delivered) mit Quality Gates und Download
- **DAG** — Interaktiver Task-Graph (Directed Acyclic Graph) mit Smoke Test, Zoom/Pan und Live-Status

### System
- **DNA Editor** — CodeMirror 6 Editor fuer System-Prompts und Konfigurationen (Auto-Save, Syntax-Highlighting)
- **Artifacts** — Projekt-Grid mit Task-Fortschritt, DAG-Ansicht und Orchestrator-Steuerung
- **Chat** — Direkte Kommunikation mit KI-Agenten (Agent-Dropdown, Chat-Verlauf, Streaming)
- **Memory** — Wissensdatenbank mit Markdown-Editor, Suche und Inline-Bearbeitung
- **Hilfe & Tutorial** — 26 Tutorial-Sektionen, 40+ FAQ-Fragen, Workflow-Anleitungen

---

## Design System

### Light & Dark Theme
Umschaltbar per Toggle in der Sidebar (Desktop) oder im Mobile-Header. Persistiert in localStorage.

- **Dark** — `#050510` Hintergrund, neon-blaue Akzente, Glassmorphism-Karten
- **Light** — `#f8f9fc` Hintergrund, Indigo-Akzente, saubere weisse Karten mit Schatten

### Projekt & Mode
Jede Ansicht ist projekt- und mode-aware:
- **Projekt-Selektor** — Wechseln zwischen Projekten (alle Daten laden projektspezifisch)
- **Mode-Toggle** — `PROD` (echte API-Calls) / `SAND` (Sandbox zum Testen ohne Kosten)

### Animationen
- Seitenuebergaenge (`hx-page-enter`)
- Button-Spring-Animation (`hx-press`)
- Hover-Lift-Effekte (`hx-hover-lift`)
- Theme-Icon-Rotation (`hx-theme-icon`)
- Respektiert `prefers-reduced-motion`

### Mobile-First
- Optimierter Header: Logo + Status + Clock + Theme-Toggle + Burger (5 Elemente)
- Projekt/Mode-Selektor im Slide-In-Menu
- Touch-optimierte Buttons und angepasste Layouts

---

## Workflow-Beispiel: SEO & GEO Workshop-PDF

```
1. Blueprint waehlen      → /blueprints → "Workshop PDF"
2. Projekt instanziieren  → Neues Projekt "seo-geo-workshop"
3. Mode waehlen           → SAND (Sandbox) fuer Testlauf
4. Agenten zuweisen       → /team → seo-geo-pro, content-writer, designer, verifier
5. DAG pruefen            → /dag → Smoke Test
6. Orchestrator starten   → /artifacts → "Run"
7. Tasks verfolgen        → /tasks → Filter: seo-geo-workshop
8. Approvals              → /approvals → CEO-Freigaben
9. Council (optional)     → /council → Multi-Agent-Beratung
10. Quality Review        → Automatisch durch verifier
11. Delivery abholen      → /deliveries → Download PDF
12. Auf PROD wechseln     → Mode → PROD → Blueprint erneut starten
13. Experiment (optional) → /experiments → A/B-Test Varianten
```

---

## Tech Stack

| Komponente | Technologie |
|-----------|-------------|
| Frontend | React 19 + Vite 6 + Tailwind CSS 4 |
| Backend | Fastify 5 + Node.js 22 (ESM) |
| KI-Gateway | OpenClaw (Chat Completions API, Port 18789) |
| Editor | CodeMirror 6 |
| Deployment | Docker Compose |
| Security | Tailscale Zero-Trust (Identity Headers) |
| VPS | Hostinger KVM-2, Ubuntu 24.04, Frankfurt |

---

## Projektstruktur

```
hypeakz-denkfabrik/
├── apps/dashboard/
│   ├── backend/                 # Fastify API Server (Port 3001)
│   │   ├── src/
│   │   │   ├── routes/          # 20+ API-Routen
│   │   │   ├── mc/              # Mission Control (Org, Skills, Work, Blueprints)
│   │   │   ├── dna/             # Agent-DNA (Fitness, Score, Routing, Mutation)
│   │   │   ├── experiments/     # A/B-Test Runner
│   │   │   ├── scheduler/       # Cron-Job Scheduler
│   │   │   └── utils/           # Atomic Writes, Locks
│   │   └── workspace/           # JSON/Markdown Datenspeicher
│   └── frontend/                # React SPA (Port 5173 dev / 8080 prod)
│       └── src/
│           ├── components/
│           │   ├── Dashboard.jsx       # Startseite mit 8 Dashboard-Panels
│           │   ├── Layout.jsx          # Sidebar, Mobile Menu, Theme Toggle
│           │   ├── FAQ.jsx             # Hilfe & Tutorial (26 Sektionen)
│           │   ├── mission/            # 18 View-Komponenten
│           │   ├── panels/             # 8 Dashboard-Panels
│           │   ├── artifacts/          # Projekt-Artefakte
│           │   ├── chat/               # Agent-Chat
│           │   ├── editor/             # DNA Editor (CodeMirror 6)
│           │   └── skills/             # Skill-Verwaltung
│           ├── lib/
│           │   ├── api.js              # API Layer (apiGet, apiPost, useProjectApi)
│           │   └── ProjectContext.jsx  # React Context (Project, Mode, Theme)
│           ├── hooks/
│           │   └── usePolling.js       # Auto-Refresh Hook
│           └── index.css               # Design System (CSS Custom Properties)
├── agents/workspace/            # OpenClaw Workspace Mapping
├── docker-compose.yml           # Development
├── docker-compose.prod.yml      # Production
├── deploy.sh                    # VPS Deployment Script
└── .env.example                 # Environment Template
```

---

## Schnellstart

### Voraussetzungen
- Node.js 22+
- Docker & Docker Compose

### Development

```bash
# 1. Repository klonen
git clone https://github.com/dankofly/denkfabrik.git
cd denkfabrik

# 2. Environment einrichten
cp .env.example .env

# 3. Mit Docker starten
docker compose up -d

# 4. Dashboard oeffnen
# Frontend: http://localhost:5173
# Backend:  http://localhost:3001
```

### Ohne Docker (manuell)

```bash
# Backend
cd apps/dashboard/backend
npm install
npm start

# Frontend (neues Terminal)
cd apps/dashboard/frontend
npm install
npm run dev
```

### Production Deployment

```bash
# Auf dem VPS:
git clone https://github.com/dankofly/denkfabrik.git /opt/hypeakz
cd /opt/hypeakz
cp .env.production.example .env
# .env anpassen (OPENCLAW_HOST, ALLOWED_TAILSCALE_USER)
docker compose -f docker-compose.prod.yml up -d --build
```

---

## Environment Variables

| Variable | Default | Beschreibung |
|----------|---------|-------------|
| `NODE_ENV` | `development` | `development` oder `production` |
| `BACKEND_PORT` | `3001` | Fastify Server Port |
| `OPENCLAW_HOST` | `http://localhost:18789` | OpenClaw Gateway URL |
| `WORKSPACE_PATH` | `./agents/workspace` | Pfad zum Workspace-Verzeichnis |
| `REQUIRE_TAILSCALE_AUTH` | `false` | Tailscale Identity-Check aktivieren |
| `ALLOWED_TAILSCALE_USER` | — | Erlaubter Tailscale-Benutzer (Production) |

---

## Sicherheit

- **Tailscale Zero-Trust** — Nur authentifizierte Geraete im Tailscale-Netzwerk haben Zugriff
- **Identity Headers** — Automatische Benutzer-Identifikation ohne Passwort
- **Docker Sandboxes** — Alle Tool-Calls laufen isoliert in Containern
- **Path-Traversal-Schutz** — Dateizugriffe auf Workspace beschraenkt
- **Input-Validierung** — Alle API-Endpunkte validieren Eingaben
- **Localhost-Binding** — Production-Ports nur auf 127.0.0.1 gebunden

---

## Agenten

| Agent | Team | Rolle |
|-------|------|-------|
| arch-dev | Dev | Development & Architecture |
| seo-geo-pro | Growth | SEO & GEO Optimization |
| revenue-bot | Sales | Revenue & Business Development |
| brand-master | Marketing | Branding & Creative |
| legal-acc | Operations | Compliance & Accounting |
| code-reviewer | Dev | Quality Assurance |
| content-writer | Marketing | Copywriting & Content |
| designer | Marketing | UI/UX Design |
| verifier | Operations | Quality Verification |
| librarian | Operations | Memory & Routing |

---

## Orchestrator

Der DAGx-Orchestrator zerlegt Projekte automatisch in Tasks und delegiert sie an Agenten:

```
Init → Dispatch → Monitor → Verify → Patch → Done
                                ↑       ↓
                                └───────┘  (bis zu 3 Korrektur-Runden)
```

**Kostenfunktion:** `J = 100 - (0.0001 × Tokens) - (0.5 × Minuten) - (10 × Fail-Rate)`

---

## Lizenz

Proprietaer. Alle Rechte vorbehalten.
