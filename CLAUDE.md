# Booking assistant

A booking system for a small business (a salon for now): a FastAPI and Postgres booking service, a React web app, an LLM chat assistant that books through tools, and an MCP server.

The full plan is in docs/blueprint.md. Read it before suggesting a design, and follow its decisions unless I say otherwise.

## How I want to work

I'm building this to learn, so I write the core code myself.

- Don't write code unless I ask for it. When I ask how to do something, explain the approach first.
- The core is mine: availability, holds, the overlap check, confirm, cancel and move, the agent loop and the tool definitions. For these, give hints and review my code instead of writing it.
- You can write boilerplate when I ask: config, Docker, migrations, CI, test fixtures, page layout. Explain each new file in a few lines.
- When I'm stuck, give me a hint or the idea first, not the full answer.
- When you review my code, point out bugs, race conditions, missing tests and simpler ways, and say why.
- Keep changes small: one step at a time.
- Ask before installing packages, deleting files or changing the database.

## Project rules

- Python 3.13 in .venv, FastAPI, SQLAlchemy 2.0, Alembic, PostgreSQL 17 in Docker, pytest.
- Store every time in UTC (timestamptz). Working hours are local clock times in America/Denver, converted date by date with zoneinfo. Never use a fixed offset.
- Time ranges are half-open: [start, end).
- Money is integer cents, never floats.
- Identity always comes from the session, never from the request body or the model.
- The model never confirms a booking. There is no confirm tool; only the customer's button calls confirm.
- Code that writes bookings locks the stylist's row (SELECT ... FOR UPDATE) before checking for overlaps.
- Every POST that creates something takes an Idempotency-Key header.
- Change the schema only through Alembic migrations.
- Secrets live in .env, which is never committed.

## Commands

- Start Postgres (from the repo root): `docker compose up -d`
- Run the API (from backend/): `fastapi dev app/main.py`
- Run the tests (from backend/): `python -m pytest`

## Progress log

At the end of each session, add a dated line to docs/progress.md: what changed, what's next, and any decision made. Don't rewrite earlier lines.
