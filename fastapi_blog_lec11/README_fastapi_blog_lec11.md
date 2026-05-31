# Lecture 11 — User Account Page & Profile Management

## Topic
Adding an authenticated account page where logged-in users can view and update their own profile information (username, email).

## What You'll Learn
- Rendering an account page accessible only to authenticated users
- Updating user profile data via a PATCH endpoint with auth protection
- Distinguishing between `UserPublic` (what others see) and `UserPrivate` (what the user sees about themselves)
- Using the `CurrentUser` dependency to identify who is making a request
- Preventing username/email conflicts when updating

## Key Concepts
| Concept | Description |
|---|---|
| `UserPrivate` | Schema exposing private fields (email, etc.) for the authenticated user |
| `UserPublic` | Schema with only public-safe fields shown to other users |
| `CurrentUser` | Dependency returning the currently authenticated user |
| Profile update | PATCH endpoint that validates uniqueness of new username/email |
| `/account` route | HTML page showing the logged-in user's details |

## File Structure
```
fastapi_blog_lec11/
├── main.py
├── auth.py              # CurrentUser dependency (updated)
├── config.py
├── routers/
│   ├── posts.py
│   └── users.py         # Account update endpoint (NEW)
├── database.py
├── models.py
├── schemas.py           # UserPublic, UserPrivate schemas (NEW)
├── templates/
│   ├── account.html     # User account/profile page (NEW)
│   ├── login.html
│   ├── register.html
│   └── ...
├── static/
└── pyproject.toml
```

## Running the App
```bash
cd fastapi_blog_lec11
uv run fastapi dev main.py
```

## Key Code Snippet
```python
# schemas.py
class UserPrivate(BaseModel):
    id: int
    username: str
    email: str  # private field — only visible to the user themselves

class UserPublic(BaseModel):
    id: int
    username: str  # public field — safe to share

# routers/users.py
@router.patch("/me", response_model=UserPrivate)
async def update_account(
    user_data: UserUpdate,
    current_user: CurrentUser,
    db: Annotated[AsyncSession, Depends(get_db)],
):
    # Validate uniqueness and apply changes
    ...
```
