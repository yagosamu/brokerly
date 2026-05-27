# Brokerly: Insurance Brokerage Management System

[![Python](https://img.shields.io/badge/Python-3.12+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Django](https://img.shields.io/badge/Django-6.0-092E20?logo=django&logoColor=white)](https://www.djangoproject.com/)
[![Bootstrap](https://img.shields.io/badge/Bootstrap-5-7952B3?logo=bootstrap&logoColor=white)](https://getbootstrap.com/)
[![SQLite](https://img.shields.io/badge/SQLite-Database-003B57?logo=sqlite&logoColor=white)](https://www.sqlite.org/)

Full-stack web application for managing the operations of an insurance brokerage: from client onboarding and proposal handling to policies, claims, endorsements, renewals, and a built-in CRM with sales pipeline. Includes an AI assistant for natural-language data queries and automated dashboard insights.

> Documentação em português disponível em [README.pt-br.md](README.pt-br.md).

---

## Screenshots

![Dashboard](docs/screenshots/dashboard_main.png)
*Admin dashboard: sales funnel, KPIs, monthly production, and portfolio distribution.*

| CRM Kanban | AI Assistant |
|:----------:|:------------:|
| ![CRM Kanban](docs/screenshots/crm-kanban.png) | ![AI Assistant](docs/screenshots/assistent_chat.png) |
| Drag-and-drop sales pipeline with per-stage totals. | Natural-language queries against the user's own data. |

| Reports | Dark Mode |
|:-------:|:---------:|
| ![Reports](docs/screenshots/reports.png) | ![Dark Mode](docs/screenshots/dark-mode.png) |
| Filterable reports with CSV and PDF export. | Built-in dark theme across the entire app. |

---

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Demo Data](#demo-data)
- [Test Users](#test-users)
- [Roles & Permissions](#roles--permissions)
- [AI Agent](#ai-agent)

---

## Features

### Core Records
- **Clients**: Individuals (PF) and companies (PJ) with full contact, address, and document data
- **Insurers**: Registration with SUSEP code, lines of business, and contacts
- **Coverages**: Insurance types, coverage definitions, and coverage items

### Operations
- **Proposals**: Full lifecycle: draft → submitted → under review → approved/rejected
- **Policies**: Validity periods, premiums, installments, commissions, and document attachments
- **Claims**: Opening, follow-up, event timeline, and documentation
- **Endorsements**: Inclusion, exclusion, modification, cancellation, and transfer
- **Renewals**: Expiration tracking with urgency and overdue alerts

### Sales
- **CRM / Kanban**: Visual pipeline with drag-and-drop (SortableJS), filters by priority and broker
- **Deals**: Full CRUD with activities (notes, calls, emails, meetings, tasks)
- **Pipelines**: Customizable stages with colors and won/lost markers

### Analytics
- **Dashboard**: KPIs, monthly production charts, distribution by type/insurer, claims overview
- **Reports**: 10 reports with filters, CSV and PDF export:
  - Production by Period
  - Commissions by Broker
  - Portfolio by Insurer
  - Portfolio by Insurance Type
  - Claims by Period
  - Loss Ratio
  - Pending Renewals
  - Clients by Broker
  - CRM Funnel
  - Endorsements by Period

### AI Agent
- **Conversational assistant**: Ask questions in natural language about your portfolio, deals, claims, and renewals
- **Auto-generated dashboard insights**: Executive summaries tailored to each user's role
- **Entity summaries**: One-click AI summaries for clients, deals, policies, proposals, and claims
- Built on LangChain + LangGraph with role-based tool access

### Administration & UI
- User CRUD with roles (Admin, Manager, Broker)
- Profile editing and password change
- DuralUX theme with complete design system
- Dark/Light mode toggle
- Permission-aware sidebar navigation
- Responsive tables, filters, and search across all lists
- Semantic status badges

---

## Tech Stack

| Layer        | Technology                              |
|--------------|------------------------------------------|
| Backend      | Python 3.12+ / Django 6.0                |
| Database     | SQLite (dev)                             |
| Frontend     | Bootstrap 5 / DuralUX template           |
| Charts       | Chart.js 4                               |
| Drag & Drop  | SortableJS                               |
| PDF Export   | xhtml2pdf                                |
| Forms        | django-widget-tweaks                     |
| Images       | Pillow                                   |
| AI / LLM     | LangChain, LangGraph, OpenAI             |

---

## Project Structure

```
brokerly/
├── accounts/          # Authentication, users, and profiles
├── ai_agent/          # LangChain-based AI assistant (chat, insights, summaries)
├── claims/            # Claims and supporting documents
├── clients/           # Clients (individuals and companies)
├── core/              # Settings, root URLs, WSGI
├── coverages/         # Insurance types and coverages
├── crm/               # Pipelines, deals, and activities
├── dashboard/         # Dashboard with KPIs and charts
├── design_system/     # Design system documentation
├── endorsements/      # Endorsements
├── insurers/          # Insurers and business lines
├── policies/          # Proposals and policies
├── renewals/          # Renewals
├── reports/           # Management reports (10 types)
├── static/            # CSS, JS, images, fonts
├── templates/         # Django templates (base, partials, per app)
├── utils/             # Mixins, validators, template tags, management commands
├── manage.py
├── requirements.txt
└── README.md
```

---

## Getting Started

### Prerequisites

- Python 3.12 or higher
- pip
- Git (optional)

### 1. Clone the repository

```bash
git clone <repo-url>
cd brokerly
```

### 2. Create and activate a virtual environment

```bash
python -m venv .venv

# macOS / Linux
source .venv/bin/activate

# Windows
.venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure environment variables

Copy the example file and edit it with your values:

```bash
cp .env.example .env
```

Available variables:

| Variable          | Required | Default                              | Description                                                  |
|-------------------|----------|--------------------------------------|--------------------------------------------------------------|
| `SECRET_KEY`      | prod     | dev-only fallback when `DEBUG=True`  | Django secret key (generate a random 50-char string)         |
| `DEBUG`           | no       | `True`                               | Enables debug mode and dev defaults                          |
| `ALLOWED_HOSTS`   | no       | `localhost,127.0.0.1`                | Comma-separated list of allowed hosts                        |
| `OPENAI_API_KEY`  | AI only  | empty                                | Required for AI Agent, insights, and summaries               |
| `OPENAI_MODEL`    | no       | `gpt-5.4`                            | OpenAI model used by the AI Agent                            |

> When `DEBUG=False`, `SECRET_KEY` **must** be provided or the app refuses to start.

### 5. Run migrations

```bash
python manage.py migrate
```

### 6. Seed demo data (recommended)

```bash
python manage.py seed_demo
```

Creates users, insurers, clients, policies, deals, and everything needed for a functional demo.

### 7. Generate AI insights (optional, requires `.env` configured)

```bash
python manage.py generate_insights

# For a single user
python manage.py generate_insights --user_id 1
```

### 8. Start the development server

```bash
python manage.py runserver
```

### 9. Open in your browser

```
http://localhost:8000
```

You'll land on the login page. Use the credentials below.

---

## Demo Data

The `seed_demo` command creates a complete realistic dataset:

```bash
# First run: creates all demo data
python manage.py seed_demo

# Re-run from a clean state
python manage.py seed_demo --clear
```

### What gets created

| Entity         | Count | Notes                                                                |
|----------------|-------|----------------------------------------------------------------------|
| Users          | 5     | 1 Admin, 1 Manager, 3 Brokers                                        |
| Insurance Types| 8     | Auto, Life, Home, Business, Health, Travel, Liability, Cargo         |
| Coverages      | ~50   | Distributed across insurance types                                   |
| Insurers       | 8     | Porto Seguro, SulAmérica, Bradesco, Allianz, Tokio, HDI, Mapfre, Liberty |
| Clients        | 20    | 12 individuals + 8 companies                                         |
| Proposals      | 28    | 20 approved, 5 rejected, 3 pending/under review                      |
| Policies       | 20    | Active, expired, and cancelled over the last 12 months               |
| Claims         | 8     | Open, under review, approved, paid, denied                           |
| Endorsements   | 6     | Inclusion, exclusion, modification, cancellation, transfer           |
| Renewals       | 10    | Pending, contacted, quote sent, renewed, not renewed                 |
| CRM Pipelines  | 2     | "New Business" (7 stages) and "Renewals" (5 stages)                  |
| Deals          | 15    | Distributed across all pipeline stages                               |
| Activities     | ~40   | Notes, calls, emails, meetings, tasks                                |

> The `--clear` flag **deletes all data** (except superusers) before recreating. Use with care.

---

## Test Users

Created automatically by `seed_demo`:

| Name                   | Email                       | Password       | Role              |
|------------------------|-----------------------------|----------------|-------------------|
| Administrador Brokerly | `admin@brokerly.com`        | `admin123`     | Admin             |
| Maria Oliveira         | `gerente@brokerly.com`      | `gerente123`   | Manager           |
| Carlos Silva           | `carlos@brokerly.com`       | `corretor123`  | Broker            |
| Ana Santos             | `ana@brokerly.com`          | `corretor123`  | Broker            |
| Rafael Pereira         | `rafael@brokerly.com`       | `corretor123`  | Broker            |

> To experience the permission system, log in as **Admin** (full access), then as **Broker** (restricted to own data).

---

## Roles & Permissions

The system defines three roles with distinct access levels:

### Admin
- Full access to all features
- User management
- All reports
- CRM pipeline management
- Sees data from all brokers

### Manager
- Full access to all operational features
- User management
- All reports
- CRM pipeline management
- Sees data from all brokers

### Broker
- Client management (own clients only)
- Proposals, policies, claims, endorsements, and renewals (own records only)
- CRM Kanban and deals (own deals only)
- No access to management reports
- No access to pipeline management
- No access to user management

---

## AI Agent

The AI Agent is a LangChain/LangGraph-based assistant integrated into the system. It exposes a chat interface, generates dashboard insights, and produces one-click summaries for any client, deal, policy, proposal, or claim.

### Key capabilities

- **Tool-restricted access**: the agent can only see data the logged-in user is allowed to see (broker tools filter by `broker=user`, admin/manager tools see everything)
- **Tools available**: client/deal/policy/proposal/claim/renewal search, summary metrics, monthly production, top insurers, commission breakdowns
- **System prompt personalization**: adapts tone and scope to the user's role and name
- **Insights generation**: `generate_insights` command produces a daily executive summary per user

### Configuration

Set `OPENAI_API_KEY` and `OPENAI_MODEL` in `.env`. Without these, the AI Agent endpoints will fail gracefully and the rest of the system continues to work.

---

## Notes

- This project was originally built as part of a Django course and has been cleaned up and extended for public sharing.
- The DuralUX admin theme (in `static/` and `design_system/refs/duralux/`) is a third-party template; copyrights and attributions in the bundled CSS/JS files are kept intact.
- Tests are scaffolded but not implemented, left as a future improvement.
