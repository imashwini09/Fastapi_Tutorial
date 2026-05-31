# Lecture 8 — APIRouter & App Modularization

## Topic
Splitting a growing single-file FastAPI app into multiple modules using `APIRouter`, keeping route logic organized by resource (posts, users).

## What You'll Learn
- Creating `APIRouter` instances in separate files (`routers/posts.py`, `routers/users.py`)
- Registering routers in the main app with `app.include_router()` and a `prefix`
- Using `tags` to group endpoints in the Swagger UI
- Keeping `main.py` clean — only app setup and UI routes live there
- Sharing dependencies (e.g., `get_db`) across routers

## Key Concepts
| Concept | Description |
|---|---|
| `APIRouter` | A mini-app that groups related routes |
| `app.include_router()` | Mounts a router onto the main app with a URL prefix |
| `prefix` | URL prefix applied to all routes in the router |
| `tags` | Swagger UI grouping label for a router's endpoints |
| `routers/` package | A Python package holding all router modules |

## File Structure
```
fastapi_blog_lec8/
├── main.py              # App setup; includes routers
├── routers/
│   ├── __init__.py
│   ├── posts.py         # Post CRUD router (NEW)
│   └── users.py         # User CRUD router (NEW)
├── database.py
├── models.py
├── schemas.py
├── templates/
├── static/
└── pyproject.toml
```

## Running the App
```bash
cd fastapi_blog_lec8
uv run fastapi dev main.py
```

## Key Code Snippet
```python
# routers/posts.py
from fastapi import APIRouter
router = APIRouter()

@router.get("", response_model=list[PostResponse])
async def get_posts(db: Annotated[AsyncSession, Depends(get_db)]):
    ...

# main.py
from routers import posts, users

app.include_router(users.router, prefix="/api/users", tags=["users"])
app.include_router(posts.router, prefix="/api/posts", tags=["posts"])
```
