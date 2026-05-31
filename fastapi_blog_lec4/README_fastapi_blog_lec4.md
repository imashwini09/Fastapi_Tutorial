# Lecture 4 — Pydantic Schemas & Request/Response Validation

## Topic
Introducing Pydantic schemas for validating incoming request bodies and shaping API responses, replacing raw Python dicts with typed models.

## What You'll Learn
- Defining Pydantic `BaseModel` classes for request (`PostCreate`) and response (`PostResponse`) shapes
- Using `response_model` on route decorators to automatically filter and validate output
- Using `Field` with constraints (e.g., `min_length`, `max_length`) for data validation
- Understanding the difference between input schemas and output schemas
- Accepting POST request bodies via Pydantic models

## Key Concepts
| Concept | Description |
|---|---|
| `BaseModel` | Pydantic class for data validation and serialization |
| `PostCreate` | Schema for validating data when creating a post |
| `PostResponse` | Schema that defines what fields are returned to the client |
| `response_model` | Tells FastAPI which schema to use for output |
| `Field(...)` | Adds constraints and metadata to schema fields |
| `ConfigDict(from_attributes=True)` | Allows Pydantic models to read from ORM objects |

## File Structure
```
fastapi_blog_lec4/
├── main.py          # Routes using response_model and request body schemas
├── schemas.py       # Pydantic models: PostCreate, PostResponse (NEW)
├── templates/
├── static/
└── pyproject.toml
```

## Running the App
```bash
cd fastapi_blog_lec4
uv run fastapi dev main.py
```

## Key Code Snippet
```python
# schemas.py
from pydantic import BaseModel, Field

class PostCreate(BaseModel):
    title: str = Field(min_length=1, max_length=100)
    content: str = Field(min_length=1)

class PostResponse(BaseModel):
    id: int
    title: str
    content: str

# main.py
@app.get("/api/posts", response_model=list[PostResponse])
def get_posts():
    return posts

@app.post("/api/posts", response_model=PostResponse, status_code=201)
def create_post(post: PostCreate):
    ...
```
