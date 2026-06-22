<div align="center">
  <img src="https://github.com/jinbocho/jinbocho-docs/blob/68902373946031d21da699b3c4ff8da513204182/assets/jinbocho-logo.png" alt="Jinbocho Logo" width="140" height="140" />
</div>

# 神保町 Jinbocho

> *Named after Tokyo's legendary booksellers' district — 古本屋街*

**Jinbocho** is a source-available home library management system designed to help families catalog, organize, and rediscover their physical book collections.

**Source-available — free for personal, non-commercial use.**

<div align="center">
  <strong>
    <a href="https://jinbocho.github.io/jinbocho-docs/">📖 Homepage</a> 
    · 
    <a href="https://jinbocho.github.io/jinbocho-demo/">🚀 Live Demo</a>
    ·
    <a href="https://jinbocho.github.io/manuals/">📘 Manuals</a>
  </strong>
</div>

---

## ✨ Features

- 📚 **Catalog your collection** — scan ISBNs, enrich metadata automatically via Open Library & Google Books
- 🗺️ **Map your shelves** — model your physical space room by room, bookcase by bookcase, section by section
- 👨‍👩‍👧 **Family-first** — multiple users per family with role-based access control (Admin, Editor, Viewer)
- 🔍 **Full-text search** — find any book by title, author, ISBN or tag with URL-synced filters
- 📤 **Export** — CSV and JSON export of your entire library
- 📖 **Reading status tracking** — track what you're reading, what's queued, what you've completed
- 📚 **Book ownership** — track who owns which copy of a book
- 👥 **Reading history** — see who has read each book across your family
- 🌍 **Multilingual UI** — English, Italian, Spanish, French with persistent user preference (backend + localStorage)
- 🔐 **Secure authentication** — JWT-based auth with refresh token rotation, password reset via email
- 📤 **Book loans** — track books lent to family members with due dates
- 📊 **Dashboard stats** — family reading trends, books by room, unread books, reading goals
- 🤖 **AI suggestions** *(in design)* — automatic tagging, duplicate detection, reading recommendations

### Why Jinbocho?

The intersection of **self-hosted + a physical map of your shelves + family sharing**
is empty. Other tools track your *reading list* or your *catalog*; none tell you the
book is *on the third shelf of the bookcase in the study*. Jinbocho does.

| | Goodreads | Libib / LibraryThing | **Jinbocho** |
|---|:---:|:---:|:---:|
| Track what you've read | ✅ | ✅ | ✅ |
| Catalog what you own | ❌ | ✅ | ✅ |
| **Physical shelf location** | ❌ | ❌ | **✅** |
| Self-hosted / your data | ❌ | ❌ | **✅** |
| Multi-user family + lending | ❌ | partial | **✅** |
| Source-available (community use) | ❌ | ❌ | **✅** |

---

## 🏗️ Architecture

Jinbocho is built as a set of small, focused microservices communicating over HTTP:

```
jinbocho-fe              → React 18 SPA (TypeScript · Vite · Tailwind CSS · TanStack Query)
  ├── 4 languages (EN/IT/ES/FR)
  ├── Mobile-first responsive design
  └── i18next + Zustand for state management

jinbocho-api-gateway-v1  → Reverse proxy, JWT validation, routing (Python · FastAPI)

jinbocho-auth-v1         → Family registration, user management, JWT + refresh tokens (Python · FastAPI)
  └── Support for future OAuth2, 2FA, SAML (auth-v2)

jinbocho-catalog-v1      → Books, locations, ISBN ingestion, search, export (Python · FastAPI)
  ├── Clean Architecture refactoring complete (Phases 0-11)
  ├── 40+ use cases, domain entities, repository pattern
  └── Ownership & reading history tracking

jinbocho-ai-v1           → AI-powered suggestions (Python · FastAPI) [in design]

jinbocho-infrastructure  → Docker Compose orchestration, environment configs
```

**All backend services** follow **Clean/Hexagonal Architecture** with:
- Domain entities and interfaces
- Application use cases (orchestration)
- Infrastructure repositories (persistence)
- API endpoints (HTTP contracts)
- Comprehensive test coverage

---

## 🚀 Quick Start

```bash
# Clone and setup
git clone https://github.com/jinbocho/jinbocho-infrastructure-v1
cd jinbocho-infrastructure-v1
./scripts/dev.sh
```

**This starts:**
- ✅ All backend microservices (Docker Compose)
- ✅ React SPA development server (Vite)
- ✅ Auto-opens browser at http://localhost:5173

**First build takes ~2 minutes.**

For detailed setup instructions, see the [Developer Manual](https://jinbocho.github.io/manuals/developer/).

---

## 🛠️ Tech Stack

**Frontend**
- React 18 · TypeScript (strict) · Vite · Tailwind CSS
- TanStack Query (server state) · Zustand (auth state) · React Router
- React Hook Form + Zod (validation) · ky (HTTP client)
- `@zxing/browser` for in-browser ISBN barcode scanning
- i18next + react-i18next for 4-language support

**Backend**
- Python 3.12+ · FastAPI · SQLAlchemy (async) · asyncpg
- Alembic (migrations) · PyJWT · passlib/bcrypt · slowapi (rate limiting)
- PostgreSQL (primary) · Supabase/Neon (cloud options)

**Infrastructure**
- Docker Compose (development)
- Kubernetes-ready (production)
- Static deploy on Render (frontend)

---

## 📊 Project Status

| Phase | Status | Scope |
|-------|--------|-------|
| **Phase 1** — Auth | ✅ **Complete** | Family registration, JWT, RBAC, password reset, token rotation |
| **Phase 2** — Catalog | ✅ **Complete** | Locations, books, ISBN ingestion, search, export, ownership, reading history |
| **Phase 3** — Gateway + Frontend | ✅ **Complete** | API gateway, React SPA (all 15+ pages built and integrated) |
| **Phase 3.5** — Internationalization | ✅ **Complete** (2026-06-10) | EN/IT/ES/FR with backend + localStorage persistence |
| **Phase 4** — AI | 💡 **In Design** | Tagging, deduplication, reading recommendations |

### Recent Milestones (June 2026)

- ✅ **Full FE rebuild** — React 18 + TypeScript strict, Vite (125 kB gzip)
- ✅ **Catalog refactoring** — 12-phase clean architecture migration (70+ files, 40+ use cases)
- ✅ **Ownership tracking** — per-member book ownership and reading history
- ✅ **Multilingual UI** — 4 languages with persistent user preference
- ✅ **Password reset** — secure email-based account recovery
- ⏳ **Phase 12 cleanup** — mypy --strict validation and final tests (in progress)

---

## 🔗 Repositories

Each service is a separate Git repository (or can be monorepo):

| Component | Language | Framework | Status |
|-----------|----------|-----------|--------|
| **Frontend** | TypeScript | React 18 + Vite | ✅ Complete |
| **Auth Service** | Python | FastAPI + PostgreSQL | ✅ Complete |
| **Catalog Service** | Python | FastAPI + PostgreSQL | ✅ Complete (Phase 12 in progress) |
| **API Gateway** | Python | FastAPI | ✅ Complete |
| **AI Service** | Python | FastAPI | 💡 In Design |
| **Infrastructure** | YAML | Docker Compose | ✅ Complete |
| **Docs** | Markdown | — | ✅ Complete |

---

## 🏫 Design Principles

- **Home-first, enterprise-ready** — runs on a Raspberry Pi today, scales to the cloud tomorrow
- **No vendor lock-in** — standard PostgreSQL, exportable data (CSV/JSON), source-available license
- **Minimal cost** — designed around free tiers; no paid services required
- **Clean architecture** — hexagonal layers, dependency inversion, testable use cases, 90%+ test coverage
- **Multilingual** — support multiple languages from day one
- **Family-centric** — designed for shared libraries and collaborative reading

---

## 🛠️ Development

Local setup, backend/frontend workflows, database migrations, CI/CD, and
troubleshooting are documented in the
[Developer Manual](https://jinbocho.github.io/manuals/developer/).

---

## 📚 Documentation

Full documentation — developer manual, user manual, and architecture — is available at
**[jinbocho.github.io/manuals](https://jinbocho.github.io/manuals/)**.

---

## 📜 License

**Source-available — free for personal, non-commercial use.**

Public repositories use the [Jinbocho Source-Available License](LICENSE):
personal, non-commercial use is free; commercial use and redistribution require permission.

This project is the exclusive intellectual property of Carmelo La Gamba.
For commercial licensing inquiries: jinbochoapp@gmail.com

---

## 🤝 Contributing

This is a source-available project. Contributions require explicit permission from the copyright holder.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

<p align="center">
  <strong>Made with ❤️ and too many unread books</strong>
  <br />
  <em>Because home libraries deserve to be seen</em>
  <br /><br />
  <a href="https://github.com/jinbocho">View on GitHub</a> •
  <a href="#quick-start">Get Started</a> •
  <a href="#documentation">Documentation</a>
</p>
