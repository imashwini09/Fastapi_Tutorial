# Lecture 1 — FastAPI Basics & Your First API

## Topic
Introduction to FastAPI: building a minimal blog API with in-memory data and a raw HTML response.

## What You'll Learn
- Installing FastAPI and running the dev server with Uvicorn
- Defining route handlers using `@app.get()`
- Returning plain `HTMLResponse` vs JSON from an endpoint
- Serving a simple in-memory list of posts over a REST API
- The `include_in_schema=False` flag to hide UI routes from the OpenAPI docs

## Key Concepts
| Concept | Description |
|---|---|
| `FastAPI()` | Creates the application instance |
| `@app.get()` | Registers a GET route |
| `HTMLResponse` | Returns raw HTML to the browser |
| In-memory data | Posts stored as a Python list of dicts |
| `/api/posts` | JSON endpoint returning all posts |

## File Structure
```
fastapi_blog/
├── main.py          # App entry point — routes and in-memory post data
├── snippets.txt     # Code snippets used during the lecture
├── pyproject.toml   # Project dependencies
└── uv.lock          # Locked dependency versions
```

## Running the App
```bash
cd fastapi_blog
uv run fastapi dev main.py
```

Then open:
- `http://localhost:8000/` — HTML home page
- `http://localhost:8000/api/posts` — JSON posts API
- `http://localhost:8000/docs` — Auto-generated Swagger UI

## Key Code Snippet
```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.get("/api/posts")
def get_posts():
    return posts  # returns JSON automatically
```
