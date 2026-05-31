# Lecture 2 — Jinja2 Templates & Static Files

## Topic
Rendering HTML pages using Jinja2 templates and serving CSS/JS/image assets with FastAPI's static file support.

## What You'll Learn
- Mounting a `StaticFiles` directory to serve CSS, JS, and icons
- Setting up `Jinja2Templates` to render `.html` template files
- Passing Python data (e.g., a list of posts) into templates via template context
- Keeping API routes (`/api/posts`) and HTML routes (`/`, `/posts`) in the same app

## Key Concepts
| Concept | Description |
|---|---|
| `StaticFiles` | Serves files from a directory under a URL prefix |
| `Jinja2Templates` | Renders HTML templates with dynamic data |
| `TemplateResponse` | Returns a rendered HTML page as a response |
| `Request` | Must be passed into template responses |

## File Structure
```
fastapi_blog_lec2/
├── main.py                  # App with static files + Jinja2 templates
├── templates/
│   ├── layout.html          # Base layout template
│   └── home.html            # Home page listing all posts
├── static/
│   ├── css/main.css         # Stylesheet
│   ├── js/utils.js          # Utility JavaScript
│   └── icons/               # Favicon and PWA icons
├── snippets.txt
└── pyproject.toml
```

## Running the App
```bash
cd fastapi_blog_lec2
uv run fastapi dev main.py
```

## Key Code Snippet
```python
from fastapi.staticfiles import StaticFiles
from fastapi.templating import Jinja2Templates

app.mount("/static", StaticFiles(directory="static"), name="static")
templates = Jinja2Templates(directory="templates")

@app.get("/")
def home(request: Request):
    return templates.TemplateResponse(request, "home.html", {"posts": posts})
```
