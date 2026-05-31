# FastAPI Tutorial — Hands-On Blog Application

A complete, progressive FastAPI tutorial series building a full-featured blog application from scratch. Each lecture folder is a standalone, working project that introduces one or more new concepts on top of the previous one.

## What You'll Build

A blog web application with:
- HTML pages rendered with Jinja2 templates
- A REST API with full CRUD for posts and users
- SQLAlchemy ORM with async PostgreSQL/SQLite support
- JWT-based authentication and authorization
- Profile picture uploads with image processing
- Email-based password reset
- Database schema migrations with Alembic

## Tech Stack

| Tool | Purpose |
|---|---|
| **FastAPI** | Web framework |
| **SQLAlchemy (async)** | ORM & database access |
| **Pydantic / pydantic-settings** | Data validation & configuration |
| **Jinja2** | HTML templating |
| **Alembic** | Database migrations |
| **Pillow** | Image processing |
| **aiosmtplib** | Async email sending |
| **PyJWT + pwdlib** | JWT tokens & password hashing |
| **uv** | Fast Python package manager |

---

## Lecture Index

| Folder | Lecture | Topic |
|---|---|---|
| [`fastapi_blog`](./fastapi_blog/) | 1 | FastAPI Basics & Your First API |
| [`fastapi_blog_lec2`](./fastapi_blog_lec2/) | 2 | Jinja2 Templates & Static Files |
| [`fastapi_blog_lec3`](./fastapi_blog_lec3/) | 3 | Path Parameters & Custom Error Handling |
| [`fastapi_blog_lec4`](./fastapi_blog_lec4/) | 4 | Pydantic Schemas & Request/Response Validation |
| [`fastapi_blog_lec5`](./fastapi_blog_lec5/) | 5 | SQLAlchemy Database Integration |
| [`fastapi_blog_lec6`](./fastapi_blog_lec6/) | 6 | Full CRUD Operations |
| [`fastapi_blog_lec7`](./fastapi_blog_lec7/) | 7 | Async SQLAlchemy & Lifespan Events |
| [`fastapi_blog_lec8`](./fastapi_blog_lec8/) | 8 | APIRouter & App Modularization |
| [`fastapi_blog_lec9`](./fastapi_blog_lec9/) | 9 | Query Parameters, Sorting & Filtering |
| [`fastapi_blog_lec10`](./fastapi_blog_lec10/) | 10 | JWT Authentication & Authorization |
| [`fastapi_blog_lec11`](./fastapi_blog_lec11/) | 11 | User Account Page & Profile Management |
| [`fastapi_blog_lec12`](./fastapi_blog_lec12/) | 12 | File Uploads & Profile Picture Processing |
| [`fastapi_blog_lec13`](./fastapi_blog_lec13/) | 13 | Pagination, Config & Database Seeding |
| [`fastapi_blog_lec14`](./fastapi_blog_lec14/) | 14 | Email Integration & Password Reset |
| [`fastapi_blog_lec15`](./fastapi_blog_lec15/) | 15 | Alembic Database Migrations |

---

## Prerequisites

- Python 3.11+
- [`uv`](https://docs.astral.sh/uv/) package manager
- Basic Python knowledge

## Getting Started

Each lecture folder is independent. Pick the one you want to study and run it:

```bash
# Navigate to any lecture folder
cd fastapi_blog_lec5

# Install dependencies
uv sync

# Run the development server
uv run fastapi dev main.py
```

Then open:
- **`http://localhost:8000`** — Blog home page
- **`http://localhost:8000/docs`** — Interactive Swagger API docs
- **`http://localhost:8000/redoc`** — Alternative ReDoc API docs

For lectures 10 and beyond, create a `.env` file in the lecture folder before running. See each lecture's README for the required variables.

---

## Learning Path

```
Lec 1  ──► FastAPI basics, inline HTML
Lec 2  ──► Add Jinja2 templates + static assets
Lec 3  ──► Path params + custom error pages
Lec 4  ──► Pydantic schemas for request/response
Lec 5  ──► SQLAlchemy ORM + dependency injection
Lec 6  ──► Full CRUD (POST / GET / PATCH / DELETE)
Lec 7  ──► Go async (AsyncSession + lifespan)
Lec 8  ──► Split into routers (modular structure)
Lec 9  ──► Query params, sorting, filtering
Lec 10 ──► JWT auth (login, protected endpoints)
Lec 11 ──► User account page + profile update
Lec 12 ──► File uploads + image processing
Lec 13 ──► Pagination + pydantic-settings config
Lec 14 ──► Email sending + password reset flow
Lec 15 ──► Alembic migrations (production-ready DB)
```

---

## Project Structure (Fully Built — Lec 15)

```
fastapi_blog_lec15/
├── main.py              # App entry point
├── auth.py              # JWT auth helpers
├── config.py            # pydantic-settings config
├── database.py          # Async SQLAlchemy engine + session
├── models.py            # ORM models (User, Post)
├── schemas.py           # Pydantic request/response schemas
├── image_utils.py       # Pillow image processing
├── email_utils.py       # Async email sending
├── populate_db.py       # Dev data seeding script
├── alembic.ini          # Alembic config
├── alembic/versions/    # Migration scripts
├── routers/
│   ├── posts.py         # Post CRUD API routes
│   └── users.py         # User/auth API routes
├── templates/           # Jinja2 HTML templates
├── static/              # CSS, JS, icons
└── media/               # Uploaded profile pictures
```

---

## Key Concepts Covered

- **Routing** — GET, POST, PATCH, DELETE with path and query params
- **Templating** — Jinja2 with layout inheritance
- **Validation** — Pydantic schemas with field constraints
- **Database** — SQLAlchemy ORM, async sessions, relationships
- **Auth** — OAuth2 + JWT with password hashing
- **File handling** — `UploadFile`, Pillow image processing, media serving
- **Config** — Environment-based settings with `pydantic-settings`
- **Email** — Async SMTP with HTML templates
- **Migrations** — Alembic for production-safe schema changes
- **Modularity** — APIRouter, dependency injection, separation of concerns
