# Lecture 6 — Full CRUD Operations

## Topic
Building complete Create, Read, Update, and Delete (CRUD) endpoints for both posts and users, using Pydantic update schemas with optional fields.

## What You'll Learn
- Implementing `POST`, `GET`, `PATCH`, and `DELETE` endpoints
- Using optional fields in update schemas (`PostUpdate`, `UserUpdate`) with `model_dump(exclude_unset=True)`
- Returning appropriate HTTP status codes (201 Created, 204 No Content, etc.)
- Filtering queries with `.where()` clauses in SQLAlchemy
- Handling "not found" cases cleanly with 404 errors

## Key Concepts
| Concept | Description |
|---|---|
| `PATCH` | Partial update — only the fields provided are changed |
| `DELETE` | Removes a resource; returns 204 No Content |
| `PostUpdate` / `UserUpdate` | Pydantic models with all-optional fields |
| `exclude_unset=True` | Only applies fields the client explicitly sent |
| `db.delete(obj)` | Marks an ORM object for deletion |

## File Structure
```
fastapi_blog_lec6/
├── main.py          # Full CRUD routes for posts and users
├── database.py
├── models.py
├── schemas.py       # Added PostUpdate and UserUpdate schemas (NEW)
├── templates/
├── static/
└── pyproject.toml
```

## Running the App
```bash
cd fastapi_blog_lec6
uv run fastapi dev main.py
```

## Key Code Snippet
```python
# schemas.py
class PostUpdate(BaseModel):
    title: str | None = Field(default=None, min_length=1, max_length=100)
    content: str | None = Field(default=None, min_length=1)

# main.py
@app.patch("/api/posts/{post_id}", response_model=PostResponse)
def update_post(post_id: int, post_data: PostUpdate, db: Session = Depends(get_db)):
    post = db.get(models.Post, post_id)
    if not post:
        raise HTTPException(status_code=404, detail="Post not found")
    for key, value in post_data.model_dump(exclude_unset=True).items():
        setattr(post, key, value)
    db.commit()
    return post

@app.delete("/api/posts/{post_id}", status_code=204)
def delete_post(post_id: int, db: Session = Depends(get_db)):
    post = db.get(models.Post, post_id)
    db.delete(post)
    db.commit()
```
