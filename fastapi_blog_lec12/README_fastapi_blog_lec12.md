# Lecture 12 — File Uploads & Profile Picture Processing

## Topic
Handling image file uploads in FastAPI, processing them with Pillow (resizing, cropping, EXIF correction), and serving uploaded media files.

## What You'll Learn
- Accepting file uploads using FastAPI's `UploadFile` and `File`
- Validating file type and size before processing
- Processing images with Pillow: EXIF-aware rotation, center-crop to square, resize, convert to JPEG
- Saving processed images to disk with unique filenames (`uuid`)
- Deleting old profile pictures when a new one is uploaded
- Serving uploaded files via a `/media` static mount

## Key Concepts
| Concept | Description |
|---|---|
| `UploadFile` | FastAPI type for handling multipart file uploads |
| `File(...)` | Form field descriptor for file uploads |
| `Pillow (PIL)` | Python image processing library |
| `ImageOps.exif_transpose` | Fixes rotation based on EXIF metadata |
| `ImageOps.fit` | Center-crops and resizes to exact dimensions |
| `uuid.uuid4().hex` | Generates a unique filename for each upload |
| `/media` mount | Serves uploaded files from the `media/` directory |

## File Structure
```
fastapi_blog_lec12/
├── main.py
├── image_utils.py       # Image processing with Pillow (NEW)
├── auth.py
├── config.py
├── routers/
│   └── users.py         # Profile picture upload endpoint (UPDATED)
├── database.py
├── models.py
├── schemas.py
├── media/
│   └── profile_pics/    # Stored uploaded profile pictures
├── templates/
│   └── account.html     # Profile page with image upload form
├── static/
└── pyproject.toml
```

## Running the App
```bash
cd fastapi_blog_lec12
uv run fastapi dev main.py
```

## Key Code Snippet
```python
# image_utils.py
def process_profile_image(content: bytes) -> str:
    with Image.open(BytesIO(content)) as original:
        img = ImageOps.exif_transpose(original)           # Fix rotation
        img = ImageOps.fit(img, (300, 300), Image.LANCZOS) # Crop to square
        if img.mode in ("RGBA", "LA", "P"):
            img = img.convert("RGB")                       # Ensure JPEG-compatible
        filename = f"{uuid.uuid4().hex}.jpg"
        img.save(PROFILE_PICS_DIR / filename, "JPEG", quality=85)
    return filename

# routers/users.py
@router.patch("/me/picture")
async def upload_profile_picture(
    file: Annotated[UploadFile, File(...)],
    current_user: CurrentUser,
    db: Annotated[AsyncSession, Depends(get_db)],
):
    content = await file.read()
    filename = process_profile_image(content)
    delete_profile_image(current_user.image_file)  # remove old
    current_user.image_file = filename
    await db.commit()
```
