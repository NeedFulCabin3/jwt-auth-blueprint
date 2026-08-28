JWT Auth Blueprint

A practical, production-ready starting point for adding secure user registration, password hashing, and JWT token authentication to FastAPI backends.

## Overview

Securing an API requires getting several moving pieces right: password salting and hashing, OAuth2-compliant token issuance, expiration management, and stateless request verification. This repository packages those core mechanisms into a single, straightforward layout. Instead of wiring up authentication from scratch for every project, you can drop this baseline directly into your backend architecture or study its mechanics.

## How It Works

1. **User Registration (`/register`)**: Accepts a username, valid email, and password. Passes the raw password through `passlib` using the `bcrypt` hashing algorithm before writing user records to storage.
2. **Authentication (`/login`)**: Takes credentials submitted via standard OAuth2 form data. Validates the plain text password against the stored `bcrypt` hash. Upon successful verification, it encodes a JSON Web Token (JWT) stamped with an expiration timestamp and signed via `HS256`.
3. **Protected Routes (`/me`)**: Guards endpoints using FastAPI's `Depends(oauth2_scheme)`. Incoming requests must supply a valid `Bearer <token>` header. The token gets decoded, checked for expiration, and resolved to the requesting user.

## Key Features

* **Bcrypt Password Security**: Hashes user credentials automatically using industry-standard schemes to prevent plaintext exposure.
* **Stateless JWT Tokens**: Issues signed access tokens using `PyJWT` with configurable expiration times (default 30 minutes).
* **Dependency Injection Protection**: Reusable endpoint guards that handle token validation and extract user context automatically.
* **Strict Schema Validation**: Built-in Pydantic models validate request payloads and sanitize outgoing JSON responses.

## Tech Stack Breakdown

* **Framework**: FastAPI
* **ASGI Server**: Uvicorn
* **Data Validation**: Pydantic (email-validator)
* **Security & Auth**: PyJWT, Passlib (Bcrypt)

## Prerequisites & Web-Based Quick Start

### Option A: Running in GitHub Codespaces (Browser Only)

1. Click the **Code** button at the top right of this repository.
2. Select the **Codespaces** tab and click **Create codespace on main**.
3. Once the cloud terminal loads, start the application:
   ```bash
   pip install fastapi uvicorn "passlib[bcrypt]" pyjwt email-validator
   python main.py
   ```
4. Click the Ports tab in the bottom panel, hover over port 8000, click the globe icon to open the app in your browser, and append /docs to the URL to interact with the interactive Swagger UI.

### Option B: Local Setup

1. Ensure Python 3.10+ is installed on your machine.
2. Clone the repository and navigate into the folder:
```text
git clone [https://github.com/your-username/fastapi-jwt-auth-blueprint.git](https://github.com/your-username/fastapi-jwt-auth-blueprint.git)
cd fastapi-jwt-auth-blueprint
```
3. Set up a virtual environment and install required packages:
```text
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install fastapi uvicorn "passlib[bcrypt]" pyjwt email-validator
```
4. Run the development server:
```text
python main.py
```
5. Open http://127.0.0.1:8000/docs in your browser to test endpoints.

## Project Structure
```text
fastapi-jwt-auth-blueprint/
├── .github/
│   └── workflows/
│       └── code-health.yml       # Automated CI pipeline for linting and code validation
├── .gitignore                    # Prevents virtual environments, caches, and secrets from committing
├── LICENSE                       # MIT open-source license permissions
├── README.md                     # Comprehensive documentation and setup guides
└── main.py                       # Core FastAPI app containing models, auth helpers, and routes
```

## Roadmap

[ ] Transition in-memory dictionary storage to an asynchronous SQLAlchemy database driver (PostgreSQL/SQLite).

[ ] Implement refresh token rotation alongside access tokens.

[ ] Add role-based access control (RBAC) scopes to route dependencies.

[ ] Environment variable integration via pydantic-settings for production secret management.

```text"Simplicity is a prerequisite for reliability." — Edsger W. Dijkstra```
