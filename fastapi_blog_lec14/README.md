# FastAPI Blog Tutorial - Lecture 14

Lecture 14 is a full async FastAPI blog app with:
- JWT auth
- user and post APIs
- server-rendered pages (Jinja2)
- password reset flow (email)
- profile image upload/processing
- pagination for posts

## Project Layout

```text
fastapi_blog_lec14/
├── main.py
├── config.py
├── database.py
├── models.py
├── schemas.py
├── auth.py
├── email_utils.py
├── image_utils.py
├── populate_db.py
├── routers/
│   ├── users.py
│   └── posts.py
├── templates/
├── static/
├── media/
├── populate_images/
├── pyproject.toml
└── uv.lock
```

## Requirements

- Python `3.12+`
- `uv` (recommended) or pip

## Environment Variables

Create a `.env` file in this folder:

```env
SECRET_KEY=change-me
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30

MAX_UPLOAD_SIZE_BYTES=5242880
POSTS_PER_PAGE=10
RESET_TOKEN_EXPIRE_MINUTES=60

MAIL_SERVER=localhost
MAIL_PORT=587
MAIL_USERNAME=
MAIL_PASSWORD=
MAIL_FROM=noreply@example.com
MAIL_USE_TLS=true

FRONTEND_URL=http://localhost:8000
```

Notes:
- `SECRET_KEY` is required.
- Password reset email uses the `MAIL_*` settings.
- Default DB is SQLite via `aiosqlite` (configured in code).

## Install Dependencies

Using `uv`:

```bash
uv sync
```

Or with pip:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -e .
```

## Run the App

```bash
uv run fastapi dev main.py
```

Alternative:

```bash
uv run uvicorn main:app --reload
```

Open:
- App UI: `http://localhost:8000`
- API docs: `http://localhost:8000/docs`

## Seed Demo Data (Optional)

This project includes a data population script with sample users, posts, and profile images:

```bash
uv run python populate_db.py
```

## Main Routes

- HTML pages:
  - `/`, `/posts`
  - `/posts/{post_id}`
  - `/users/{user_id}/posts`
  - `/login`, `/register`, `/account`
  - `/forgot-password`, `/reset-password`
- APIs:
  - `/api/users/*`
  - `/api/posts/*`

## What This Lecture Demonstrates

- Async SQLAlchemy usage in FastAPI
- Token auth and protected endpoints
- Password reset token lifecycle
- Background email sending
- File upload handling with Pillow
- Paginated API and paginated UI behavior
