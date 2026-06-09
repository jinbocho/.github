# 神保町 Jinbocho

> *Named after Tokyo's legendary booksellers' district — 古本屋街*

**Jinbocho** is an open-source home library management system designed to help families catalog, organize, and rediscover their physical book collections.

---

## What is Jinbocho?

Most home libraries are invisible. Books pile up on shelves with no record of what you own, where it lives, or whether you've read it. Jinbocho fixes that.

- 📚 **Catalog your collection** — scan ISBNs, enrich metadata automatically via Open Library & Google Books
- 🗺️ **Map your shelves** — model your physical space room by room, bookcase by bookcase
- 👨‍👩‍👧 **Family-first** — multiple users per family, role-based access
- 🔍 **Full-text search** — find any book by title, author, ISBN or tag
- 📤 **Export** — CSV and JSON export of your entire library
- 📖 **Reading status** — track what you're reading, what's queued, what's done
- 🤖 **AI suggestions** *(coming soon)* — automatic tagging, duplicate detection, reading recommendations

---

## Architecture

Jinbocho is built as a set of small, focused services communicating over HTTP:

```
jinbocho-fe          → React SPA (TypeScript · Vite · Tailwind)
jinbocho-api-gateway → Reverse proxy, JWT validation, routing
jinbocho-auth        → Family registration, user management, JWT issuance
jinbocho-catalog     → Books, locations, ingestion, search, export
jinbocho-ai          → AI-powered suggestions (Phase 4)
jinbocho-infrastructure → Docker Compose, environment configs
jinbocho-docs        → Architecture decisions, DB schema, requirements
```

All backend services are Python/FastAPI with PostgreSQL, following **Clean/Hexagonal Architecture**.

---

## Repositories

| Repository | Description | Stack |
|---|---|---|
| [`jinbocho-auth-v1`](https://github.com/jinbocho/jinbocho-auth-v1) | Authentication & user management | Python · FastAPI · PostgreSQL |
| [`jinbocho-catalog-v1`](https://github.com/jinbocho/jinbocho-catalog-v1) | Library catalog, locations, ingestion | Python · FastAPI · PostgreSQL |
| [`jinbocho-api-gateway-v1`](https://github.com/jinbocho/jinbocho-api-gateway-v1) | API gateway & routing | Python · FastAPI |
| [`jinbocho-ai-v1`](https://github.com/jinbocho/jinbocho-ai-v1) | AI suggestions *(Phase 4)* | Python · FastAPI |
| [`jinbocho-fe`](https://github.com/jinbocho/jinbocho-fe) | Web frontend (SPA) | React 18 · TypeScript · Tailwind |
| [`jinbocho-infrastructure-v1`](https://github.com/jinbocho/jinbocho-infrastructure-v1) | Docker Compose, envs | Docker |
| [`jinbocho-docs`](https://github.com/jinbocho/jinbocho-docs) | Architecture docs, ADRs, DB schema | Markdown |

---

## Tech Stack

**Backend**
- Python 3.12 · FastAPI · SQLAlchemy (async) · asyncpg
- Alembic (migrations) · PyJWT · passlib/bcrypt · slowapi

**Frontend**
- React 18 · TypeScript (strict) · Vite · Tailwind CSS
- TanStack Query · Zustand · React Router · ky · React Hook Form + Zod
- `@zxing/browser` for in-browser ISBN barcode scanning
- Deployed on [Render](https://render.com) (static site)

**Infrastructure**
- PostgreSQL · Docker Compose
- Free tiers: [Supabase](https://supabase.com) · [Neon](https://neon.tech) (cloud) or local Docker (home server)

---

## Principles

- **Home-first, enterprise-ready** — runs on a Raspberry Pi today, scales to the cloud tomorrow
- **No vendor lock-in** — standard PostgreSQL, exportable data, AGPL license
- **Minimal cost** — designed around free tiers; no paid services required to run it yourself
- **Clean architecture** — hexagonal layers, dependency inversion, testable use cases

---

## Roadmap

| Phase | Status | Scope |
|---|---|---|
| **Phase 1** — Auth | ✅ Complete | Family registration, JWT, RBAC, token rotation |
| **Phase 2** — Catalog | ✅ Complete | Locations, books, ISBN ingestion, search, export |
| **Phase 3** — Gateway + Frontend | ✅ Complete | API gateway, React SPA (all pages built and wired) |
| **Phase 4** — AI | 💡 Future | Tagging, dedup, reading recommendations |

---

## License

All Jinbocho repositories are licensed under **[AGPL-3.0](https://www.gnu.org/licenses/agpl-3.0.html)**.  
Any network-deployed modifications must be published as open source.

---

<p align="center">
  Made with ❤️ and too many unread books
</p>
