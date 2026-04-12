# Petstore QA Playground

Practice project for **OOP API client design, contract testing, and test automation** using the Swagger Petstore API.

Swagger specification
https://petstore.swagger.io/v2/swagger.json

---

# Current Status

Stage 1 — OOP API Client (in progress)
Last updated: 2026-04-12

Planned stages:

1. OOP Petstore client
2. Create Makefile with `check` target
3. Push project to GitHub using `gh`
4. Generate Postman collection
5. Build pytest automation suite
6. Add GitHub Actions CI

---

# Project Context

Owner is a **Senior QA Engineer learning Python automation**.

Background

* Manual QA
* API testing (Postman)
* SQL
* Test strategy

Python is a **learning area**.

Guidelines when writing code:

* Prefer **clarity over cleverness**
* Add **short comments explaining non-obvious Python**
* When introducing a new concept (fixtures, pydantic, parametrization), briefly explain **why it is used**

Keep explanations short (2–3 sentences).

---

# Tech Stack (Strict)

Python 3.11+

HTTP

* `httpx` (synchronous client)

Models

* `pydantic v2`

Testing

* `pytest`
* `pytest-html`

Configuration

* `python-dotenv`

Code quality

* `ruff`
* `mypy --strict`

Do not introduce new frameworks without approval.

Forbidden tools:

* `requests`
* `unittest`
* `Flask`
* `FastAPI`
* async code

Reason: async adds complexity and is **not part of the current learning goal**.

---

# Architecture

Clean OOP API client design.

Project structure

```
client/
  base.py
  constants.py
  exceptions.py

  resources/
    pet.py
    store.py
    user.py

  models/
    pet_models.py
    store_models.py
    user_models.py

tests/
  conftest.py
  test_pet_resource.py
```

---

# Layer Responsibilities

### client/base.py

Central HTTP client responsible for:

* httpx client
* base URL
* retries
* logging
* error handling

Expose main entrypoint:

```
PetstoreClient
```

Example usage

```
client.pet.get_pet(10)
```

---

### client/resources/

One class per Swagger tag.

Example

```
class PetResource:
```

Responsibilities

* endpoint methods
* request serialization
* response validation

---

### client/models/

Pydantic models mirroring Swagger schemas.

Benefits

* request validation
* response validation
* contract testing
* type safety

---

### tests/

Tests interact with the API **through the client layer**.

Bad

```
httpx.get(...)
```

Good

```
client.pet.get_pet()
```

Tests verify **behavior**, not HTTP implementation.

---

# Error Handling

Never return `None` on API failure.

Always raise typed exceptions:

```
PetstoreAPIError
PetstoreNotFound
PetstoreValidationError
```

Explicit failures make negative testing and debugging easier.

---

# Coding Standards

Type hints required.

Naming conventions

```
snake_case → functions
PascalCase → classes
UPPER_SNAKE_CASE → constants
```

Docstrings: Google style.

Import order

1. standard library
2. third-party libraries
3. local modules

---

# Testing Strategy

Use pytest with the AAA pattern.

```
Arrange
Act
Assert
```

Tests should cover

* happy path
* contract validation
* negative responses (4xx)
* edge cases

Client setup should be handled through **pytest fixtures in `conftest.py`**.

---

# Workflow Rules

### Plan Mode

For tasks touching multiple files:

1. Enter **Plan Mode**
2. Show implementation plan
3. Wait for approval before coding

Example plan

```
1. Implement constants
2. Implement PetstoreClient
3. Implement PetResource
4. Add models
5. Add tests
```

---

### Small Diffs

Default to **small, reviewable diffs**.

If a change exceeds ~150 lines, pause and propose splitting it into smaller commits.

---

### Test Planning

Before writing tests:

Generate a **test list first** and confirm.

Example

```
Test Plan for PetResource

- test_add_pet_success
- test_get_pet_success
- test_get_pet_not_found
- test_update_pet_success
- test_delete_pet_success
```

---

# Quality Gate

Before declaring a task complete run

```
make check
```

The Makefile will run

```
ruff check .
mypy .
pytest -q
```

Report results after running.

---

# Security

Never commit secrets.

```
.env → gitignored
.env.example → committed
```

---

# When Unsure

If a requirement is unclear or conflicts with existing code:

**Stop and ask one specific question instead of guessing.**

---

# What This Project Is NOT

This project is

* not production infrastructure
* not a reusable public library

Primary goal is **learning test architecture and Python automation**.
