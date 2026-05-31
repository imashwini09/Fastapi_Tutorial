# Lecture 10 — JWT Authentication & Authorization

## Topic
Securing the API with JWT (JSON Web Token) based authentication: password hashing, token creation, protected endpoints, and OAuth2 login flow.

## What You'll Learn
- Hashing passwords using `pwdlib` before storing in the database
- Implementing a `/token` endpoint using `OAuth2PasswordRequestForm`
- Creating JWT access tokens with expiry using `PyJWT`
- Protecting routes with `OAuth2PasswordBearer` and a `get_current_user` dependency
- Restricting operations (create, update, delete) to authenticated users only
- Storing secrets (secret key, algorithm, expiry) in a config file

## Key Concepts
| Concept | Description |
|---|---|
| `pwdlib` | Password hashing library (bcrypt-backed) |
| `OAuth2PasswordBearer` | FastAPI security scheme for Bearer tokens |
| `OAuth2PasswordRequestForm` | Parses `username` + `password` from a form POST |
| `jwt.encode` / `jwt.decode` | Creates and verifies JWT tokens |
| `get_current_user` | Dependency that decodes the token and returns the user |
| `CurrentUser` | Annotated type alias for the authenticated user dependency |

## File Structure
```
fastapi_blog_lec10/
├── main.py              # App with protected routes
├── auth.py              # Password hashing, JWT creation/verification (NEW)
├── config.py            # Settings: secret key, algorithm, expiry (NEW)
├── routers/
│   ├── posts.py         # Protected post routes (require auth)
│   └── users.py         # /token login endpoint + user CRUD
├── database.py
├── models.py
├── schemas.py           # Token schema added
├── templates/
│   ├── login.html       # Login form (NEW)
│   └── register.html    # Registration form (NEW)
├── static/
└── pyproject.toml
```

## Running the App
```bash
cd fastapi_blog_lec10
# Set up your .env with SECRET_KEY first
uv run fastapi dev main.py
```

## Environment Variables (`.env`)
```
SECRET_KEY=your-secret-key-here
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
DATABASE_URL=sqlite+aiosqlite:///./blog.db
```

## Key Code Snippet
```python
# auth.py
def create_access_token(data: dict) -> str:
    to_encode = data.copy()
    to_encode["exp"] = datetime.now(UTC) + timedelta(minutes=settings.access_token_expire_minutes)
    return jwt.encode(to_encode, settings.secret_key.get_secret_value(), algorithm=settings.algorithm)

# routers/users.py
@router.post("/token")
async def login(form: OAuth2PasswordRequestForm = Depends(), db: AsyncSession = Depends(get_db)):
    user = await authenticate_user(form.username, form.password, db)
    token = create_access_token({"sub": str(user.id)})
    return {"access_token": token, "token_type": "bearer"}
```
