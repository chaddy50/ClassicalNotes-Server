# AGENTS.md

ClassicalNotes-Server: a FastAPI + PostgreSQL REST API for logging classical music concert attendance — performances with ordered set lists of works, performers (orchestras, ensembles, conductors, soloists, choruses), composers, and venues. Backend for the [ClassicalNotes](https://github.com/chaddy50/ClassicalNotes) Android client.

## Stack

- FastAPI, served by Uvicorn
- SQLAlchemy ORM + PostgreSQL (via psycopg2), migrations via Alembic
- Pydantic for request validation and response serialization
- pytest + HTTPX (FastAPI's test client) against an in-memory SQLite database for tests
- Docker + Compose for self-hosting

## Layout

- `app/models/` — SQLAlchemy models, one file per resource (performance, performer, work, venue, composer, set list entry), plus shared base classes (`base_schema.py`, `client_supplied_id.py`) and `enums.py`.
- `app/routers/` — FastAPI routers, one per resource, mirroring `app/models/`.
- `alembic/` — database migrations; `alembic.ini` at the repo root configures Alembic.
- `tests/` — pytest suite, one `test_*.py` per resource, `conftest.py` for shared fixtures.
- `seeding/`, `seed.py` — local data seeding for development.

## Commands

- `pip install -r requirements.txt` — install dependencies.
- `pytest` — run the test suite (mirrors CI's `test` job in `.github/workflows/test.yml`, which runs on push).
- `alembic revision --autogenerate -m "<message>"` / `alembic upgrade head` — after changing `app/models/`.

## Conventions

- One router + one SQLAlchemy model per resource; new resources should follow the existing model/router pairing.
- Tests run against an in-memory SQLite database via HTTPX and FastAPI's test client, not the real Postgres — keep new tests consistent with the fixtures in `conftest.py`.
- Requests/responses are validated via Pydantic schemas, not raw dicts.

## Git & Commits

- **Never include Claude (or any AI assistant) as a commit co-author or contributor.** No `Co-Authored-By: Claude` trailer, no "Generated with Claude Code" line, no assistant mention in commit messages, PR titles, or PR descriptions. Write commits as the author, describing the change and why.
- Create commits only when explicitly asked.
