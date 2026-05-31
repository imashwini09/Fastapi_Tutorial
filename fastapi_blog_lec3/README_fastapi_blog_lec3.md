# Lecture 3 — Path Parameters & Custom Error Handling

## Topic
Adding individual post pages via path parameters, and building custom HTTP exception handlers that render friendly error pages instead of raw JSON errors.

## What You'll Learn
- Using path parameters (`/posts/{post_id}`) to look up individual resources
- Raising `HTTPException` with appropriate status codes (e.g., 404)
- Overriding FastAPI's default error handlers with `@app.exception_handler()`
- Handling both `StarletteHTTPException` and `RequestValidationError`
- Returning custom HTML error pages from exception handlers

## Key Concepts
| Concept | Description |
|---|---|
| `{post_id}` path param | Captures a segment of the URL as a typed variable |
| `HTTPException` | Raises an HTTP error with a status code and detail message |
| `exception_handler` | Decorator to register a custom error response |
| `StarletteHTTPException` | Base class for all HTTP errors in the stack |
| `RequestValidationError` | Raised when request body/query params fail validation |

## File Structure
```
fastapi_blog_lec3/
├── main.py                  # Routes + custom exception handlers
├── templates/
│   ├── layout.html
│   ├── home.html
│   ├── post.html            # Single post detail page (NEW)
│   └── error.html           # Custom error page (NEW)
├── static/
└── pyproject.toml
```

## Running the App
```bash
cd fastapi_blog_lec3
uv run fastapi dev main.py
```

## Key Code Snippet
```python
from fastapi import HTTPException, status
from starlette.exceptions import HTTPException as StarletteHTTPException

@app.get("/posts/{post_id}", include_in_schema=False)
def post_page(request: Request, post_id: int):
    for post in posts:
        if post.get("id") == post_id:
            return templates.TemplateResponse(request, "post.html", {"post": post})
    raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail="Post not found")

@app.exception_handler(StarletteHTTPException)
def http_exception_handler(request: Request, exception: StarletteHTTPException):
    return templates.TemplateResponse(request, "error.html", {"message": exception.detail})
```
