# FastAPI Blog Lesson: Lecture 13

## Overview
This lesson demonstrates a complete FastAPI blog application with user authentication, database integration, and image upload functionality. It covers authentication using JWT tokens, SQLAlchemy ORM, and file upload handling.

## Prerequisites
*   Python 3.x
*   FastAPI installed
*   PostgreSQL or SQLite database
*   Basic understanding of FastAPI, SQLAlchemy, and JWT authentication

## Setup
Follow the setup instructions in the main project README or the setup guide for this tutorial.

## Running the Code
To run the example code, follow these steps:
1.  Activate the virtual environment: `source .venv/bin/activate`
2.  Run the main application file: `python main.py`

## What You Will Learn
*   **User Authentication**: Implement JWT-based authentication with password hashing
*   **Database Integration**: Use SQLAlchemy ORM for database operations
*   **Image Upload**: Handle profile image uploads with PIL processing
*   **API Design**: Create RESTful APIs for blog posts and users
*   **Dependency Injection**: Use FastAPI's dependency injection for database sessions and authentication
*   **Security**: Implement secure password handling and token verification

## Key Files
*   `main.py` - Application entry point with router configuration
*   `auth.py` - Authentication logic with JWT token creation and verification
*   `config.py` - Application configuration settings
*   `models.py` - SQLAlchemy models for User and Post
*   `routers/users.py` - User management API endpoints
*   `routers/posts.py` - Blog post CRUD operations
*   `schemas.py` - Pydantic schemas for request/response validation
*   `image_utils.py` - Image processing utilities

## Dependencies
*   FastAPI
*   SQLAlchemy
*   Pydantic
*   Pydantic Settings
*   JWT (pyjwt)
*   Password hashing (pwdlib)
*   PIL (Pillow) for image processing
*   Starlette

## Next Steps
*   Explore the related lectures in the FastAPI blog series.
*   Experiment with the concepts covered in this lesson.
*   Customize the blog application with your own content.