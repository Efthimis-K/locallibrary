# Local Library

A Django-based local library catalog application.

## Overview

This project implements a small library management system with models for books, authors, genres, and book instances. It is intended as a local catalog for tracking available copies and loan status.

## Features

- Book listing and detail views
- Author listing and detail views
- Genre filtering
- Book instance tracking
- Admin interface for managing catalog data

## Setup

1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Apply database migrations:
   ```bash
   python manage.py migrate
   ```
3. Create a superuser:
   ```bash
   python manage.py createsuperuser
   ```
4. Run the development server:
   ```bash
   python manage.py runserver
   ```

## Notes

- The project uses the default SQLite database for local development.
- Update the README with additional details as the application evolves.
