<div align="center">
  <img src="https://github.com/jinbocho/jinbocho-docs/blob/68902373946031d21da699b3c4ff8da513204182/assets/jinbocho-logo.png" alt="Jinbocho Logo" width="140" height="140" />
</div>

# 神保町 Jinbocho

> *Named after Tokyo's legendary booksellers' district — 古本屋街*

**Jinbocho** is a source-available home library management system that helps
families and shared households catalog, organize, and rediscover their
physical book collections — down to the exact shelf a book is sitting on.

**Source-available — free for personal, non-commercial use.**

<div align="center">
  <strong>
    <a href="https://jinbocho.github.io">Homepage</a>
    ·
    <a href="https://jinbocho.github.io/jinbocho-demo/">Live Demo</a>
    ·
    <a href="https://jinbocho.github.io/pricing/">Pricing</a>
    ·
    <a href="https://jinbocho.github.io/manuals/">Manuals</a>
  </strong>
</div>

---

## Features

- **Catalog your collection** — scan ISBNs or import a Goodreads CSV export; metadata is enriched automatically via Open Library and Google Books
- **Shelf Scan** *(Pro, AI)* — photograph a bookshelf and have every spine read, matched, and catalogued already positioned on that exact shelf; Shelf Audit later re-checks a shelf against the catalog
- **Map your shelves** — model your physical space room by room, bookcase by bookcase, section by section
- **Shared libraries** — multiple members per household with role-based access (Admin, Editor, Viewer)
- **Search, ratings & wishlist** — full-text search with URL-synced filters, star ratings with household-wide stats, a pre-purchase wishlist
- **Reading life** — reading status, per-member reading history, loans with due-date reminders, list or cover-grid browsing
- **Export anytime** — CSV and JSON export of your entire library, no lock-in
- **AI suggestions** *(Pro)* — automatic tagging, duplicate detection, reading recommendations, AI-written incipits
- **Multilingual UI** — English, Italian, Spanish, French, with persistent user preference
- **Secure authentication** — JWT with refresh-token rotation, password reset via email

### Why Jinbocho?

The intersection of **self-hosted + a physical map of your shelves + shared access**
is empty. Other tools track your *reading list* or your *catalog*; none tell you
the book is *on the third shelf of the bookcase in the study* — or let you
photograph that shelf and have it cataloged for you.

| | Goodreads | Libib / LibraryThing | **Jinbocho** |
|---|:---:|:---:|:---:|
| Track what you've read | ✅ | ✅ | ✅ |
| Catalog what you own | ❌ | ✅ | ✅ |
| **Physical shelf location** | ❌ | ❌ | **✅** |
| **Photo-to-catalog (Shelf Scan)** | ❌ | ❌ | **✅** *(Pro)* |
| Self-hosted / your data | ❌ | ❌ | **✅** |
| Multi-user household + lending | ❌ | partial | **✅** |
| Source-available (community use) | ❌ | ❌ | **✅** |

---

## Architecture

Jinbocho is built as a set of small, focused microservices communicating over HTTP:

```
jinbocho-fe                            React 18 SPA — TypeScript · Vite · Tailwind CSS · TanStack Query
                                        4 languages (EN/IT/ES/FR), mobile-first, barcode + shelf-photo capture

jinbocho-api-gateway-v1                Reverse proxy, JWT validation, routing (Python · FastAPI)

jinbocho-auth-v1                       Accounts, users, roles, JWT + refresh tokens (Python · FastAPI)

jinbocho-catalog-v1                    Books, locations, ISBN ingestion, shelf-scan orchestration,
                                        search, export (Python · FastAPI)
                                        Clean/Hexagonal architecture, 50+ use cases, repository pattern

jinbocho-ai-v1                         AI features (Python · FastAPI): incipit generation, tag
                                        suggestions, recommendations, duplicate detection, vision-based
                                        shelf scan — cleanly disabled when no LLM key is configured

jinbocho-infrastructure-community-v1   Docker Compose orchestration, GHCR images, one-command VPS install
jinbocho-infrastructure-pro-v1         AI module overlay (licensed)
```

**All backend services** follow **Clean/Hexagonal Architecture** with:
- Domain entities and interfaces
- Application use cases (orchestration)
- Infrastructure repositories and external-service adapters
- API endpoints (HTTP contracts)
- Comprehensive test coverage

---

## Quick Start

```bash
git clone https://github.com/jinbocho/jinbocho-infrastructure-community-v1.git
cd jinbocho-infrastructure-community-v1
sudo ./scripts/setup-vps-community.sh --domain your-domain.com --email you@example.com
```

Pre-built images from GHCR, automatic TLS via Caddy, no source build required —
online in a few minutes. Omit `--domain`/`--email` to run over plain HTTP for
local testing.

Prefer to try it before installing anything? Use the [Live Demo](https://jinbocho.github.io/jinbocho-demo/).
For local development, source builds, or the optional AI module, see the
[Developer Manual](https://jinbocho.github.io/manuals/developer/).

---

## Tech Stack

**Frontend**
- React 18 · TypeScript (strict) · Vite · Tailwind CSS
- TanStack Query (server state) · Zustand (auth state) · React Router
- React Hook Form + Zod (validation) · ky (HTTP client)
- `@zxing/browser` for in-browser ISBN barcode scanning
- i18next + react-i18next for 4-language support

**Backend**
- Python 3.12+ · FastAPI · SQLAlchemy (async) · asyncpg
- Alembic (migrations) · PyJWT · passlib/bcrypt · slowapi (rate limiting)
- PostgreSQL — self-hosted or managed, one database per service

**Infrastructure**
- Docker Compose, one-command self-host on any VPS · Kubernetes-ready
- GHCR-published images, built and pushed on every release
- Opt-in observability: Sentry-compatible error tracking, OpenTelemetry tracing, Prometheus + Grafana

---

## Project Status

| Phase | Status | Scope |
|-------|--------|-------|
| **Phase 1** — Auth | Complete | Registration, JWT, RBAC, password reset, token rotation |
| **Phase 2** — Catalog | Complete | Locations, books, ISBN ingestion, search, export, ownership, reading history |
| **Phase 3** — Gateway + Frontend | Complete | API gateway, React SPA (all pages built and integrated) |
| **Phase 3.5** — Internationalization | Complete | EN/IT/ES/FR with backend + localStorage persistence |
| **Phase 4** — AI | Complete | Incipit generation, tag suggestions, recommendations, duplicate detection, Shelf Scan + Shelf Audit |

### Recent Milestones (2026-07)

- **Shelf Scan** — photograph a shelf, AI reads the spines, books are catalogued already positioned there; **Shelf Audit** reconciles a shelf against the catalog
- **Ratings & Wishlist** — star ratings with household stats, a separate pre-purchase wishlist
- **Goodreads CSV import** — preview/confirm import with automatic dedup
- **Cover grid view** — gallery browsing alongside the existing list view
- **Loans UX** — dashboard visibility, overdue badges, per-book reminders
- **User avatars** — profile photos across sidebar, stats, and member views
- **Observability rollout (Phase 1)** — error tracking and uptime monitoring across all four services

**In progress**: PWA installability, guided onboarding with sample data, series/collection grouping.

---

## Repositories

Each service is a separate Git repository:

| Component | Language | Framework | Status |
|-----------|----------|-----------|--------|
| **Frontend** | TypeScript | React 18 + Vite | Complete |
| **Auth Service** | Python | FastAPI + PostgreSQL | Complete |
| **Catalog Service** | Python | FastAPI + PostgreSQL | Complete |
| **API Gateway** | Python | FastAPI | Complete |
| **AI Service** | Python | FastAPI + PostgreSQL | Complete — Pro/licensed |
| **Infrastructure (Community)** | YAML | Docker Compose | Complete — public |
| **Infrastructure (Pro)** | YAML | Docker Compose overlay | Complete — licensed |
| **Docs** | Markdown | — | Complete |

---

## Design Principles

- **Home-first, enterprise-ready** — runs on a Raspberry Pi or a small VPS today, scales to Kubernetes tomorrow
- **No vendor lock-in** — standard PostgreSQL, exportable data (CSV/JSON), source-available license
- **AI is optional, never required** — every AI-backed feature degrades cleanly and disappears from the UI when no LLM key is configured
- **Clean architecture** — hexagonal layers, dependency inversion, testable use cases
- **Multilingual** — support multiple languages from day one
- **Shared by design** — built for households and small groups, not just individuals

---

## Documentation

Full documentation — developer manual, user manual, and architecture — is available at
**[jinbocho.github.io/manuals](https://jinbocho.github.io/manuals/)**.

---

## License

**Source-available — free for personal, non-commercial use.**

Public repositories use the [Jinbocho Source-Available License](LICENSE):
personal, non-commercial use is free; commercial use, redistribution, and
hosted/managed services require permission. The optional AI module
(`jinbocho-ai-v1`) is distributed separately under a Pro license — see
[Pricing](https://jinbocho.github.io/pricing/).

This project is the exclusive intellectual property of Carmelo La Gamba.
For commercial licensing inquiries: jinbochoapp@gmail.com

---

## Contributing

This is a source-available project — contributions require explicit permission
from the copyright holder before you start work. Open an issue or write to
jinbochoapp@gmail.com to discuss a change before submitting a pull request.

---

<p align="center">
  <strong>Made with too many unread books</strong>
  <br />
  <em>Because home libraries deserve to be seen</em>
  <br /><br />
  <a href="https://github.com/jinbocho">View on GitHub</a> ·
  <a href="#quick-start">Get Started</a> ·
  <a href="#documentation">Documentation</a>
</p>
