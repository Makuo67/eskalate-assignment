# AI Experts Assignment – OAuth Client

This repository implements a lightweight HTTP client with OAuth2 token handling, plus a test suite and Docker configuration suitable for a CI environment.

## Getting Started (Local Implementation)

1. Create & activate a virtual environment
   (Linux)

```
python3 -m venv <eskalate> # choose a name for the venv
source <eskalate>/bin/activate
```

Windows

```
.\venv\Scripts\activate
```

2. Install dependencies

```
pip install -r requirements.
```

3. Run tests locally

```
pytest -v
```

## Running Tests with Docker

1. Build the Docker image

```
docker build -t client-tests .
```

2. Run the test suite inside Docker

```
docker run --rm client-tests
```

This ensures tests run in a clean, reproducible environment identical to CI.

### requirements.txt

Contains only the minimal pinned dependencies used by the application and tests.

### Explanation.md

Describes the identified bug, its cause, and the reasoning behind the fix.
