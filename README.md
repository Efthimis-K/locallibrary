# Local Library

A Django-based library management web application, built as part of the Mozilla Developer Network (MDN) Django tutorial. The project manages a local library's catalog of books, authors, genres, and book copies (instances), including borrowing and return workflows.

## Tech Stack

| Component      | Technology                                           |
| -------------- | ---------------------------------------------------- |
| Framework      | Django 5.2                                           |
| Language       | Python 3                                             |
| Database       | SQLite (development), PostgreSQL via dj-database-url |
| Static files   | WhiteNoise                                           |
| Env management | python-dotenv                                        |
| Deployment     | Gunicorn                                             |

## Features

- **Book listing and detail views** — Browse the full catalog with pagination.
- **Author listing and detail views** — Browse authors with pagination.
- **Genre filtering** — Books are categorized by genre.
- **Book instance tracking** — Track individual copies (imprint, status, due date, borrower).
- **Borrowing workflow** — Users can view their currently borrowed books; librarians can view all borrowed books.
- **Renewal** — Librarians can extend the due date on borrowed books.
- **Authentication & permissions** — Login required for borrowing views; `can_mark_returned` permission for librarian actions.
- **Author CRUD** — Create, update, and delete authors (with permission checks).
- **Admin interface** — Full admin panel for managing catalog data.
- **Session-based visit counter** — Tracks number of visits on the home page.

## Data Model

- **Genre** — Book genre (e.g., Science Fiction). Case-insensitive unique constraint on `name`.
- **Book** — Book metadata (title, summary, ISBN, FK to Author, M2M to Genre). ISBN is unique.
- **BookInstance** — Specific copy of a book (UUID primary key, borrower, due date, status, imprint). Supports `is_overdue` property and custom permission `can_mark_returned`.
- **Author** — Author information (first/last name, birth/death dates). Ordered by last name.

## URL Routes

| Route                      | View                        | Name                   | Notes                          |
| -------------------------- | --------------------------- | ---------------------- | ------------------------------ |
| `/`                        | `index`                     | `index`                | Home page with stats           |
| `/books/`                  | `BookListView`              | `books`                | Paginated book list            |
| `/book/<int:pk>`           | `BookDetailView`            | `book-detail`          | Book detail                    |
| `/authors/`                | `AuthorListView`            | `authors`              | Paginated author list          |
| `/author/<int:pk>`         | `AuthorDetailView`          | `author-detail`        | Author detail                  |
| `/mybooks/`                | `LoanedBooksByUserListView` | `my-borrowed`          | Current user's borrowed books  |
| `/borrowed/`               | `LoanedBooksAllListView`    | `all-borrowed`         | All borrowed books (librarian) |
| `/book/<uuid:pk>/renew/`   | `renew_book_librarian`      | `renew-book-librarian` | Renew a book (librarian)       |
| `/author/create/`          | `AuthorCreate`              | `author-create`        | Create author                  |
| `/author/<int:pk>/update/` | `AuthorUpdate`              | `author-update`        | Update author                  |
| `/author/<int:pk>/delete/` | `AuthorDelete`              | `author-delete`        | Delete author                  |

## Project Structure

```
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
│   ├── forms.py               # Django forms (e.g., RenewBookForm)
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
├── .gitignore
├── AGENTS.md
├── LICENSE
└── README.md
```

## Environment Variables

| Variable            | Purpose                        | Default                |
| ------------------- | ------------------------------ | ---------------------- |
| `DJANGO_SECRET_KEY` | Django secret key              | Required (no fallback) |
| `DJANGO_DEBUG`      | Debug mode (`False` disables)  | Enabled by default     |
| `DATABASE_URL`      | Override DB (e.g., PostgreSQL) | SQLite                 |

A `.env` file is loaded via python-dotenv in `settings.py`.

## Setup

1. **Clone the repository:**

   ```bash
   git clone https://github.com/Efthimis-K/locallibrary.git
   cd locallibrary
   ```

2. **Create and activate a virtual environment:**

   ```bash
   python -m venv .venv
   .venv\Scripts\Activate.ps1   # Windows PowerShell
   # or: source .venv/bin/activate  # macOS/Linux
   ```

3. **Install dependencies:**

   ```bash
   pip install -r requirements.txt
   ```

4. **Set environment variables:**
   Create a `.env` file in the project root with:

   ```
   DJANGO_SECRET_KEY=your-secret-key-here
   DJANGO_DEBUG=True
   ```

5. **Apply database migrations:**

   ```bash
   python manage.py migrate
   ```

6. **Create a superuser:**

   ```bash
   python manage.py createsuperuser
   ```

7. **Run the development server:**

   ```bash
   python manage.py runserver
   ```

8. **Open the application:**
   Visit http://127.0.0.1:8000/ in your browser.

## Running Tests

```bash
python manage.py test
```

## Notes

- The project uses the default SQLite database for local development. Set `DATABASE_URL` in `.env` to use PostgreSQL in production.
- Static files are served by WhiteNoise in production.
- User-facing strings and comments are in English.
