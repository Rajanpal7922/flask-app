# Flask App

A simple Flask web application with GitHub Actions CI/CD pipeline integration.

---

##  Overview

This repository contains a minimal Flask application designed as a starting point for web development with Python. It includes:

- A simple route handler
- CI/CD integration using GitHub Actions
- Optional Docker support for containerized deployment

---

##  Project Structure

```text
flask-app/
│
├── app.py              # Main Flask application
├── requirements.txt    # Python dependencies
├── README.md           # Project documentation
├── .github/            # GitHub Actions workflow files (for CI/CD)
│   └── workflows/
│       └── ci-cd.yml   # CI/CD pipeline configuration
└── Dockerfile          # Containerization setup (optional)
```

---

##  Installation

Follow these steps to set up the Flask app on your local machine:

### 1. Clone the Repository

```bash
git clone https://github.com/Rajanpal7922/flask-app.git
cd flask-app
```

### 2. Create and Activate a Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate    # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the Flask App

```bash
python app.py
```

Visit `http://localhost:5000/` in your browser.

---

##  Architecture Diagram

```
+-------------+          +-------------+          +----------------+          +--------------------+
| User Browser|  ---->   | Flask Server|  ---->   | Route Handler  |  ---->   | Response Sent Back  |
+-------------+          +-------------+          +----------------+          +--------------------+
      |                       |                          |                            |
 Sends HTTP request     Receives request          Executes code               Returns HTML/text
 (e.g. GET /)           and routes it             for that route             response to browser
                        to handler
```

---

##  GitHub Actions Workflow (CI/CD)

This project includes a GitHub Actions workflow for automatic build and test.

### Location: `.github/workflows/ci-cd.yml`

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run tests
        run: |
          # Add your test commands here, e.g. pytest
          echo "No tests defined"

      # Optional deployment steps go here
```

> You can customize this workflow by adding your test commands or deployment steps as needed.

---

##  Docker Support (Optional)

To build and run the app using Docker:

```bash
docker build -t flask-app .
docker run -p 5000:5000 flask-app
```

Then open `http://localhost:5000/` in your browser.

---
