# Roll Call Attendance App 🎓
<div align="left">
    <img src="https://img.shields.io/badge/Python-FFD43B?style=for-the-badge&logo=python&logoColor=blu"/>
    <img src="https://img.shields.io/badge/PostgreSQL-green?style=for-the-badge"/>
    <img src="https://img.shields.io/badge/fastapi-109989?style=for-the-badge&logo=FASTAPI&logoColor=white"/>
    <img src="https://img.shields.io/badge/Pydantic-E92063?style=for-the-badge&logo=Pydantic&logoColor=white"/>
    <img src="https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=JSON%20web%20tokens&logoColor=white"/>
    <img src="https://img.shields.io/badge/PyTest-0A9EDC?style=for-the-badge&logo=pytest&logoColor=white"/>
</div>

A secure attendance-management web application for university classes. Professors can create sections, open attendance sessions, and review attendance or absence requests, while students can confirm presence with a code or QR token, request excused absences, and track their attendance status over time.

## Features

- Role-based access for professors and students
- Attendance sessions with expiring codes and QR-based sign-in
- Optional location validation for attendance confirmations
- Late attendance and excused absence handling
- Absence request workflow with attachments
- Notifications for class cancellations and approval decisions
- Attendance statistics and per-session reporting
- Browser UI served from the FastAPI app with static frontend assets
- PostgreSQL-backed persistence with SQLAlchemy models

## Tech Stack

- Python 3.11+
- FastAPI
- SQLAlchemy 2
- PostgreSQL
- Pydantic Settings
- JWT-based authentication
- pwdlib for password hashing
- pytest for automated tests
- Docker and Docker Compose for local development

## Project Structure

```text
.
├── app/                     # FastAPI application
│   ├── __init__.py
│   ├── config.py            # Environment settings and app config
│   ├── db.py                # SQLAlchemy engine/session setup
│   ├── main.py              # Application routes and business logic
│   ├── models.py            # Database models
│   ├── schemas.py           # API schemas / request and response models
│   ├── security.py          # Auth, token creation, code validation, session flow
│   └── static/              # Browser UI assets (HTML, CSS, JS)
├── docs/                    # Project documentation and guides
│   ├── user-guide.md
│   ├── presenter-guide.md
│   ├── ui-api.md
│   ├── ui-developer-guide.md
│   └── attendance-*.md / *.puml
├── tests/                   # Pytest suite for API and behavior validation
├── .env.example             # Sample environment variables
├── .gitignore
├── Dockerfile
├── docker-compose.yml
├── pyproject.toml
├── uv.lock
├── PROBLEMS.md
├── plan.md
├── plan(1).md
├── plan(2).md
└── README.md                # This file
```

## Getting Started 🟢

### Option 1: Docker Compose (recommended)

This repository includes a Docker Compose setup for the app and PostgreSQL database.

1. Copy the example environment file:
   ```bash
   cp .env.example .env
   ```

2. Start the stack:
   ```bash
   docker compose up --build
   ```

3. Open:
   ```text
   http://localhost:8000/
   ```

The default Compose setup runs:
- API on port 8000
- PostgreSQL database on the internal Docker network
- Uploads stored in a Docker volume

### Option 2: Local Python environment

1. Create and activate a virtual environment:
   ```bash
   python -m venv .venv
   source .venv/bin/activate
   ```

2. Install dependencies:
   ```bash
   pip install .
   ```

3. Set environment variables:
   ```bash
   export DATABASE_URL=postgresql+psycopg://attendance:attendance-dev-only@localhost:5432/attendance
   export JWT_SECRET=replace-with-a-long-random-secret
   export CODE_SECRET=replace-with-a-different-long-random-secret
   export PROFESSOR_SIGNUP_CODE=replace-with-a-code-you-give-to-professors
   export UPLOAD_DIR=./uploads
   export APP_ENV=development
   ```

4. Start the app:
   ```bash
   uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
   ```

## Environment Variables ⚙️

The app reads configuration through `app/config.py` and supports the following settings:

- `DATABASE_URL` — database connection string
- `JWT_SECRET` — secret key for JWT token signing
- `CODE_SECRET` — secret key used for attendance code and QR validation
- `ACCESS_TOKEN_MINUTES` — lifetime of auth tokens
- `QR_TTL_SECONDS` — lifetime of QR tokens
- `PROFESSOR_SIGNUP_CODE` — shared code used for professor sign-up
- `UPLOAD_DIR` — directory for uploaded attachments
- `MAX_UPLOAD_BYTES` — maximum file size for uploaded documents
- `APP_ENV` — set to `production` to reject weak default secrets

## Running Tests

```bash
pytest
```

The repository includes tests covering:
- attendance flows
- absence tracking and statistics
- auth and authorization
- security controls
- UI endpoints
- section handling
- review/fix regression scenarios

## Security Notes

This project includes security-focused behavior such as:

- JWT-based session auth
- password hashing via `pwdlib`
- short-lived QR tokens and hashed attendance codes
- rate limiting settings for login and attendance checks
- optional location checks during attendance confirmation
- HTTP-only session cookies for browser UI auth
- production-mode checks to refuse weak development secrets

Use strong secrets in non-development environments and avoid committing real credentials.

## Documentation

Additional project guidance is available in:

- `docs/user-guide.md` — student and professor usage guide
- `docs/presenter-guide.md` — presenter/operator guidance
- `docs/ui-api.md` — UI/API information
- `docs/attendance-database.md` — schema and database design notes

## License
TBA
