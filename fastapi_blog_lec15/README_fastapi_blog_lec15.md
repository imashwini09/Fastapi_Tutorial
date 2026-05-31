# Lecture 15 — Alembic Database Migrations

## Topic
Managing database schema changes safely over time using Alembic, FastAPI's recommended migration tool for SQLAlchemy — no more `create_all()` wiping your data.

## What You'll Learn
- Setting up Alembic in a FastAPI project (`alembic init`, `alembic.ini`, `env.py`)
- Writing migration scripts: initial schema creation and adding new columns
- Running migrations with `alembic upgrade head` and rolling back with `alembic downgrade`
- Removing `Base.metadata.create_all()` from app startup (Alembic owns migrations now)
- Adding a `likes` column to the `Post` model as a practical migration example

## Key Concepts
| Concept | Description |
|---|---|
| Alembic | SQLAlchemy migration framework — tracks and applies schema changes |
| `alembic init` | Initializes Alembic config and `alembic/` directory |
| `alembic revision --autogenerate` | Auto-generates a migration from model changes |
| `alembic upgrade head` | Applies all pending migrations |
| `alembic downgrade -1` | Reverts the last migration |
| `versions/` | Folder storing all migration scripts |

## File Structure
```
fastapi_blog_lec15/
├── main.py              # Startup no longer calls create_all (UPDATED)
├── alembic.ini          # Alembic configuration file (NEW)
├── alembic/
│   ├── env.py           # Migration environment — imports your models
│   ├── script.py.mako   # Template for new migration scripts
│   └── versions/
│       ├── 1d613fe39e51_initial_schema.py       # First migration (NEW)
│       └── 39402126ec37_add_likes_to_posts.py   # Add likes column (NEW)
├── auth.py
├── config.py
├── email_utils.py
├── image_utils.py
├── routers/
├── database.py
├── models.py            # Post model now has `likes` field
├── schemas.py
├── templates/
├── static/
└── pyproject.toml
```

## Running Migrations
```bash
cd fastapi_blog_lec15

# Apply all migrations to set up the database
alembic upgrade head

# After changing a model, generate a new migration
alembic revision --autogenerate -m "describe your change"

# Roll back the last migration
alembic downgrade -1

# Then start the app (no create_all needed)
uv run fastapi dev main.py
```

## Key Code Snippet
```python
# alembic/versions/39402126ec37_add_likes_to_posts.py
def upgrade() -> None:
    op.add_column("post", sa.Column("likes", sa.Integer(), nullable=False, server_default="0"))

def downgrade() -> None:
    op.drop_column("post", "likes")

# main.py lifespan (create_all REMOVED)
@asynccontextmanager
async def lifespan(_app: FastAPI):
    yield                     # Alembic handles schema setup
    await engine.dispose()
```
