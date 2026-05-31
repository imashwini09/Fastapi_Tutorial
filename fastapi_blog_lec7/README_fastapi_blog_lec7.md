# Lecture 7 — Async SQLAlchemy & Lifespan Events

## Topic
Migrating from synchronous to fully asynchronous database access using `AsyncSession`, and managing app startup/shutdown with FastAPI's lifespan context manager.

## What You'll Learn
- Converting synchronous SQLAlchemy ORM to `AsyncSession` and `AsyncEngine`
- Writing `async def` route handlers with `await` for all DB calls
- Using `asynccontextmanager` and the `lifespan` parameter for startup/shutdown logic
- Loading related models eagerly with `selectinload` to avoid lazy-load issues in async context
- How async improves throughput under concurrent load

## Key Concepts
| Concept | Description |
|---|---|
| `AsyncSession` | SQLAlchemy async session for non-blocking DB operations |
| `async def` / `await` | Python async syntax for non-blocking calls |
| `lifespan` | FastAPI's replacement for deprecated `on_startup` / `on_shutdown` events |
| `selectinload` | Eager-loads related objects in a separate async query |
| `asynccontextmanager` | Wraps setup and teardown code around a `yield` |

## File Structure
```
fastapi_blog_lec7/
├── main.py          # Fully async routes + lifespan startup (NEW)
├── database.py      # AsyncEngine, AsyncSessionLocal, async get_db (UPDATED)
├── models.py        # Same ORM models, compatible with async
├── schemas.py
├── templates/
├── static/
└── pyproject.toml
```

## Running the App
```bash
cd fastapi_blog_lec7
uv run fastapi dev main.py
```

## Key Code Snippet
```python
from contextlib import asynccontextmanager
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy.orm import selectinload

@asynccontextmanager
async def lifespan(_app: FastAPI):
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.create_all)
    yield
    await engine.dispose()

app = FastAPI(lifespan=lifespan)

@app.get("/api/posts")
async def get_posts(db: Annotated[AsyncSession, Depends(get_db)]):
    result = await db.execute(
        select(models.Post).options(selectinload(models.Post.author))
    )
    return result.scalars().all()
```
