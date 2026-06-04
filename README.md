# Python CI/CD Project with GitHub Actions

## Project Overview

This project demonstrates a simple Python application integrated with GitHub Actions for Continuous Integration (CI).

Whenever code is pushed to the `main` branch, GitHub Actions automatically:

* Checks out the repository
* Sets up Python
* Installs dependencies
* Runs automated tests using Pytest

This helps ensure code quality and prevents bugs from being introduced into the project.

---

## Project Structure

```text
my-python-project/
│
├── src/
│   ├── addition.py
│   └── test_addition.py
│
└── .github/
    └── workflows/
        └── python-ci.yml
```

---

## Application Code

### addition.py

```python
def add(a, b):
    return a + b
```

This function takes two numbers and returns their sum.

---

## Test Cases

### test_addition.py

```python
from addition import add

def test_add():
    assert add(1, 2) == 3
    assert add(1, -1) == 0
```

The test file verifies that the addition function returns the expected results.

---

## GitHub Actions Workflow

The workflow automatically runs whenever code is pushed to the repository.

### Workflow Steps

1. Checkout repository code
2. Install Python
3. Install Pytest
4. Execute test cases
5. Report build status

---

## Running Locally

Install Pytest:

```bash
pip install pytest
```

Run tests:

```bash
cd src
pytest test_addition.py -v
```

---

## CI Pipeline Flow

```text
Developer Pushes Code
          │
          ▼
GitHub Actions Triggered
          │
          ▼
Python Environment Setup
          │
          ▼
Install Dependencies
          │
          ▼
Run Pytest Test Cases
          │
          ▼
Build Passes or Fails
```

---

## Technologies Used

* Python
* Pytest
* GitHub Actions
* Git
* GitHub

---

## Author

Harsh Saini

This project was created to learn Continuous Integration (CI) using GitHub Actions and automated testing with Pytest.
