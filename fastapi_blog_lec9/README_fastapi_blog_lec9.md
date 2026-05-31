# Lecture 9 — Query Parameters, Sorting & Filtering

## Topic
Adding query parameter support for sorting posts and filtering content, demonstrating how FastAPI automatically parses URL query strings into typed function parameters.

## What You'll Learn
- Declaring query parameters as function arguments (e.g., `sort: str = "desc"`)
- Applying `.order_by()` to SQLAlchemy queries based on query params
- Filtering posts by `user_id` or other criteria via query strings
- Combining query parameters with path parameters in the same route
- Validating query param values (e.g., only allow `"asc"` or `"desc"`)

## Key Concepts
| Concept | Description |
|---|---|
| Query parameters | URL parameters after `?` (e.g., `?sort=asc`) |
| Default values | Query params with defaults are optional |
| `.order_by(desc(...))` | SQLAlchemy ordering by column |
| Posts sorted by date | `order_by(models.Post.date_posted.desc())` |
| Combined filtering | Chain `.where()` and `.order_by()` on the same query |

## File Structure
```
fastapi_blog_lec9/
├── main.py              # Home route now orders posts by date desc
├── routers/
│   ├── posts.py         # Post routes with sort/filter query params
│   └── users.py         # User routes
├── database.py
├── models.py
├── schemas.py
├── templates/
├── static/
└── pyproject.toml
```

## Running the App
```bash
cd fastapi_blog_lec9
uv run fastapi dev main.py
```

## Key Code Snippet
```python
@app.get("/")
async def home(request: Request, db: Annotated[AsyncSession, Depends(get_db)]):
    result = await db.execute(
        select(models.Post)
        .options(selectinload(models.Post.author))
        .order_by(models.Post.date_posted.desc()),  # newest first
    )
    posts = result.scalars().all()
    return templates.TemplateResponse(request, "home.html", {"posts": posts})

# Query param example in a router
@router.get("")
async def get_posts(sort: str = "desc", user_id: int | None = None, ...):
    ...
```
