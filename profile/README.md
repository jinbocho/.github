# 神保町 Jinbocho

> *Named after Tokyo's legendary booksellers' district — 古本屋街*

**Jinbocho** is an open-source home library management system designed to help families catalog, organize, and rediscover their physical book collections.

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
- 🤖 **AI suggestions** *(coming soon)* — automatic tagging, duplicate detection, reading recommendations

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

jinbocho-ai-v1           → AI-powered suggestions (Python · FastAPI) [Phase 4 — future]

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
git clone https://github.com/carmelolg/workspace-jinbocho
cd workspace-jinbocho
./start.sh
```

**This starts:**
- ✅ All backend microservices (Docker Compose)
- ✅ React SPA development server (Vite)
- ✅ Auto-opens browser at http://localhost:5173

**First build takes ~2 minutes.**

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
| **AI Service** | Python | FastAPI | 💡 Planned |
| **Infrastructure** | YAML | Docker Compose | ✅ Complete |
| **Docs** | Markdown | — | ✅ Complete |

---

## 🏫 Design Principles

- **Home-first, enterprise-ready** — runs on a Raspberry Pi today, scales to the cloud tomorrow
- **No vendor lock-in** — standard PostgreSQL, exportable data (CSV/JSON), AGPL license
- **Minimal cost** — designed around free tiers; no paid services required
- **Clean architecture** — hexagonal layers, dependency inversion, testable use cases, 90%+ test coverage
- **Multilingual** — support multiple languages from day one
- **Family-centric** — designed for shared libraries and collaborative reading

---

## 🛠️ Development

### Local Setup

```bash
# Start everything
./start.sh

# Or manually:
cd jinbocho-infrastructure-v1
docker compose up -d --build

# Run migrations
docker compose exec auth-service alembic upgrade head
docker compose exec catalog-service alembic upgrade head

# Start frontend
cd ../jinbocho-fe
npm install && npm run dev
```

### Type Checking & Tests

```bash
# Frontend
cd jinbocho-fe
npm run typecheck  # TypeScript strict mode
npm run test       # Unit tests

# Backend (example: auth service)
cd jinbocho-auth-v1
python -m mypy app --strict     # Type checking
python -m pytest tests/ -v      # Test suite
python -m ruff check app tests  # Linting
```

---

## 📚 Documentation

- **Architecture** — see `/jinbocho-docs/architecture/`
- **API** — see OpenAPI Swagger at `http://localhost:8000/docs` (when running)
- **Database schema** — see `/jinbocho-docs/`
- **Refactoring plans** — see `/jinbocho-claude/BACKLOG_CATALOG.md` (Phase 12 cleanup in progress)

---

## 📜 License

All Jinbocho repositories are licensed under **[AGPL-3.0](https://www.gnu.org/licenses/agpl-3.0.html)**.

Any network-deployed modifications must be published as open source.

---

## 🎯 Roadmap

**Upcoming (Phase 4)**
- AI service for intelligent tagging
- Duplicate book detection
- Personalized reading recommendations
- Advanced search filters (by condition, source, date range)

**Future (Phase 5+)**
- Mobile app (React Native)
- Book clubs and reading groups
- Integration with Goodreads/OpenLibrary (bidirectional sync)
- Advanced analytics and reading trends
- Social features (family sharing, wishlists)

---

## 🤝 Contributing

This is an open-source project licensed under AGPL-3.0. Contributions are welcome!

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
  <a href="https://github.com/carmelolg/workspace-jinbocho">View on GitHub</a> •
  <a href="#quick-start">Get Started</a> •
  <a href="#documentation">Documentation</a>
</p>
