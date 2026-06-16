# Project: Local Library (django_local_library)

## Overview

A Django-based library management web application, built as part of the Mozilla Developer Network (MDN) Django tutorial. The project manages a local library's catalog of books, authors, genres, and book copies (instances), including borrowing and return workflows.

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Framework | Django 5.2.6 |
| Language | Python 3.13 |
| Database | SQLite (development), supports PostgreSQL via `DATABASE_URL` env variable |
| Static files | WhiteNoise + Django statics |
| Env management | python-dotenv |
| Deployment | Gunicorn ready |
| Dev tooling | django-debug-toolbar |

Key dependencies are pinned in `requirements.txt`.

---

## Project Structure

```text
locallibrary/                  # Project root / Django project config
├── locallibrary/
│   ├── settings.py            # Django settings (env vars, middleware, DB, static files)
│   ├── urls.py                # Root URL config
│   ├── wsgi.py / asgi.py
├── catalog/                   # Main app
│   ├── models.py              # Genre, Book, BookInstance, Author
│   ├── views.py               # Book/author list/detail views, borrowing views, CRUD views
│   ├── urls.py                # App URL patterns
│   ├── admin.py               # Admin registrations
│   ├── forms.py               # Django forms
│   ├── migrations/            # DB migrations
│   ├── templates/             # HTML templates (app-specific)
│   ├── static/css/styles.css  # Custom stylesheet
│   └── tests/
│       ├── test_models.py
│       ├── test_views.py
│       └── test_forms.py
├── templates/
│   └── registration/          # Auth templates (login, logout, password reset)
├── manage.py
├── db.sqlite3                 # Local SQLite database
├── requirements.txt
└── README.md
```

---

## Data Model Summary

- **Genre**:책 장르 (e.g., Science Fiction). Case-insensitive unique constraint on `name`.
- **Book**:책 정보 (title, summary, ISBN, FK to Author, M2M to Genre). ISBN is unique.
- **BookInstance**: 특정 도서 복사본 (UUID primary key, borrower, due date, status, imprint). Supports `is_overdue` property and custom permission `can_mark_returned`.
- **Author**: 작가 정보 (first/last name, birth/death dates). Ordered by last name.

---

## URL Routes (catalog/urls.py)

- `/` — index
- `/books/` — Book list
- `/book/<int:pk>` — Book detail
- `/authors/` — Author list
- `/author/<int:pk>` — Author detail
- `/mybooks/` — 현재 사용자가 대출한 책 목록
- `/borrowed/` — 전체 대출 목록 (librarian view)
- `/book/<uuid:pk>/renew/` — 사서가 대출 기한 연장
- `/author/create/` — Author 생성
- `/author/<int:pk>/update/` — Author 수정
- `/author/<int:pk>/delete/` — Author 삭제

---

## Environment Variables

| Variable | Purpose | Default |
|-----------|---------|---------|
| `DJANGO_SECRET_KEY` | Django secret key | development fallback |
| `DJANGO_DEBUG` | Debug mode (`False` disables) | development fallback |
| `DATABASE_URL` | Override DB (e.g., PostgreSQL) | SQLite |

`.env` file is loaded via python-dotenv in `settings.py`.

---

## Running Locally

```powershell
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

Tests:

```powershell
python manage.py test
```

---

## Notes for Agents

- Django 5.2 codebase; do not use deprecated APIs.
- Keep templates in `templates/` and `catalog/templates/catalog/`.
- Static assets live under `catalog/static/`; WhiteNoise handles static file serving in production.
- Local DB file `db.sqlite3` is present in the root; do not commit.
- When adding models or migrations, follow the existing pattern in `catalog/models.py` and `catalog/migrations/`.
- URL names should remain backward-compatible where possible (existing views use named URLs for `get_absolute_url`).
- User-facing strings and comments should remain in English to match the codebase.
