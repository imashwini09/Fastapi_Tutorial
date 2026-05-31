# Lecture 13 — Pagination, Config & Database Seeding

## Topic
Implementing post pagination, centralizing app configuration with `pydantic-settings`, and seeding the database with realistic sample data for development.

## What You'll Learn
- Using `pydantic-settings` to load config from `.env` files (type-safe, validated)
- Implementing cursor-based / limit-based pagination with SQLAlchemy (`LIMIT`, `OFFSET`, `COUNT`)
- Building a `populate_db.py` script to seed users and posts for development
- Using `httpx` for async HTTP calls within the seeding script (simulating real API calls)
- Configuring `posts_per_page` and `max_upload_size_bytes` from environment

## Key Concepts
| Concept | Description |
|---|---|
| `pydantic-settings` | Loads and validates settings from env vars / `.env` files |
| `BaseSettings` | Pydantic model for configuration |
| `SettingsConfigDict` | Configures env file path and encoding |
| `SecretStr` | Hides sensitive config values from logs |
| Pagination | `LIMIT` + `COUNT` to return a page of results and a `has_more` flag |
| `populate_db.py` | Script to seed the DB with test users and posts |

## File Structure
```
fastapi_blog_lec13/
├── main.py              # Home route uses posts_per_page from config
├── config.py            # Pydantic BaseSettings (NEW)
├── populate_db.py       # DB seeding script with sample users/posts (NEW)
├── populate_images/     # Sample images used by the seeding script
├── auth.py
├── routers/
├── database.py
├── models.py
├── schemas.py
├── media/
├── templates/
├── static/
└── pyproject.toml
```

## Running the App
```bash
cd fastapi_blog_lec13
# Create .env first (see below)
uv run fastapi dev main.py

# Seed the database with sample data
uv run python populate_db.py
```

## Environment Variables (`.env`)
```
DATABASE_URL=sqlite+aiosqlite:///./blog.db
SECRET_KEY=your-secret-key
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
MAX_UPLOAD_SIZE_BYTES=5242880
POSTS_PER_PAGE=10
```

## Key Code Snippet
```python
# config.py
class Settings(BaseSettings):
    model_config = SettingsConfigDict(env_file=".env")
    secret_key: SecretStr
    posts_per_page: int = 10
    max_upload_size_bytes: int = 5 * 1024 * 1024

settings = Settings()

# main.py — paginated home
result = await db.execute(
    select(models.Post)
    .order_by(models.Post.date_posted.desc())
    .limit(settings.posts_per_page),
)
```
