# Lecture 14 — Email Integration & Password Reset

## Topic
Adding email sending functionality to the blog, including a complete forgot-password / reset-password flow using time-limited JWT tokens and `aiosmtplib`.

## What You'll Learn
- Sending emails asynchronously with `aiosmtplib`
- Rendering HTML email templates with Jinja2
- Implementing a secure password reset flow:
  1. User requests a reset → server emails a signed token link
  2. User clicks the link → server validates the token
  3. User submits a new password → server updates the hash
- Generating short-lived JWT tokens specifically for password reset
- Adding email-related settings to the config (`MAIL_SERVER`, `MAIL_PORT`, etc.)

## Key Concepts
| Concept | Description |
|---|---|
| `aiosmtplib` | Async SMTP library for sending emails |
| `EmailMessage` | Python stdlib class for building email messages |
| HTML email | Jinja2-rendered template sent as `text/html` alternative |
| Password reset token | Short-lived JWT (`sub` = user email, `purpose` = "reset") |
| `forgot_password` flow | POST email → validate → reset via token link |

## File Structure
```
fastapi_blog_lec14/
├── main.py
├── email_utils.py       # Email sending helpers (NEW)
├── image_utils.py
├── auth.py
├── config.py            # Mail server settings added (UPDATED)
├── routers/
│   └── users.py         # Forgot/reset password endpoints (NEW)
├── database.py
├── models.py
├── schemas.py
├── templates/
│   ├── email/
│   │   └── password_reset.html   # HTML email template (NEW)
│   ├── forgot_password.html      # Request reset form (NEW)
│   └── reset_password.html       # Set new password form (NEW)
├── static/
└── pyproject.toml
```

## Running the App
```bash
cd fastapi_blog_lec14
uv run fastapi dev main.py
```

## Additional Environment Variables (`.env`)
```
MAIL_SERVER=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=your@email.com
MAIL_PASSWORD=your-app-password
MAIL_USE_TLS=true
MAIL_FROM=your@email.com
FRONTEND_URL=http://localhost:8000
```

## Key Code Snippet
```python
# email_utils.py
async def send_password_reset_email(to_email: str, username: str, token: str):
    reset_url = f"{settings.frontend_url}/reset-password?token={token}"
    html_content = templates.env.get_template("email/password_reset.html").render(
        reset_url=reset_url, username=username
    )
    await send_email(to_email, "Reset Your Password", plain_text=..., html_content=html_content)

# routers/users.py
@router.post("/forgot-password")
async def forgot_password(email: str, db: AsyncSession = Depends(get_db)):
    user = await get_user_by_email(email, db)
    token = create_access_token({"sub": user.email, "purpose": "reset"}, expires_delta=timedelta(hours=1))
    await send_password_reset_email(user.email, user.username, token)
```
