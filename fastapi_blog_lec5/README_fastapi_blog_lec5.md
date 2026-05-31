# Lecture 5 — SQLAlchemy Database Integration

## Topic
Replacing in-memory data with a real SQLite/PostgreSQL database using SQLAlchemy ORM and FastAPI's dependency injection system.

## What You'll Learn
- Setting up SQLAlchemy with `engine`, `SessionLocal`, and `Base`
- Defining ORM models (`Post`, `User`) that map to database tables
- Using `Base.metadata.create_all()` to auto-create tables on startup
- Injecting a database session into route handlers with `Depends(get_db)`
- Querying the database using SQLAlchemy's `select()` statement

## Key Concepts
| Concept | Description |
|---|---|
| `engine` | SQLAlchemy connection to the database |
| `Base` | Declarative base for ORM model definitions |
| `SessionLocal` | Factory for creating database sessions |
| `get_db()` | Dependency that yields a DB session and ensures it's closed |
| `Depends(get_db)` | FastAPI dependency injection for DB sessions |
| `db.execute(select(...))` | Runs a SELECT query via SQLAlchemy |

## File Structure
```
fastapi_blog_lec5/
├── main.py          # Routes using DB session dependency
├── database.py      # Engine, Base, SessionLocal, get_db (NEW)
├── models.py        # SQLAlchemy ORM models: Post, User (NEW)
├── schemas.py       # Pydantic schemas (updated for DB)
├── templates/
│   └── user_posts.html   # User's post listing page (NEW)
├── static/
└── pyproject.toml
```

## Running the App
```bash
cd fastapi_blog_lec5
uv run fastapi dev main.py
```

## Key Code Snippet
```python
# database.py
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, DeclarativeBase

engine = create_engine("sqlite:///./blog.db")
SessionLocal = sessionmaker(bind=engine)

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

# main.py
from sqlalchemy import select
from sqlalchemy.orm import Session

@app.get("/api/posts")
def get_posts(db: Annotated[Session, Depends(get_db)]):
    result = db.execute(select(models.Post))
    return result.scalars().all()
```
