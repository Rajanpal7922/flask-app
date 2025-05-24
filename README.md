# Flask App

A simple Flask web application with GitHub Actions CI/CD pipeline integration.

---

## Overview

This repository contains a minimal Flask application designed as a starting point for web development with Python. It includes:

- A simple route handler
- CI/CD integration using GitHub Actions
- Optional Docker support for containerized deployment

---

## Project Structure

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

## Installation

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

## Architecture Diagram

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

## GitHub Actions Workflow (CI/CD)

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
```

---

## Deployment

This project uses **Docker-based deployment to an AWS EC2 instance** via **GitHub Actions**.

### Deployment Workflow Summary

1. **GitHub Actions** is triggered on every push to the `master` branch.
2. It **builds a Docker image** of the Flask app using `docker/build-push-action`.
3. The image is **pushed to DockerHub** under your account.
4. GitHub Actions then uses **SSH** to connect to your EC2 instance.
5. It **pulls the latest Docker image** and **runs it as a container** on EC2.

### GitHub Secrets Required

Make sure you define the following secrets in your GitHub repository under:
**Settings → Secrets and Variables → Actions**

- `DOCKER_USERNAME`: Your DockerHub username
- `DOCKER_PASSWORD`: Your DockerHub password or token
- `EC2_HOST`: The public IP address of your EC2 instance
- `EC2_USER`: The SSH username (e.g., `ec2-user` or `ubuntu`)
- `SSH_PRIVATE_KEY`: Your EC2 private key used to SSH into the instance

### Workflow File: `.github/workflows/deploy.yml`

```yaml
name: Build, Push and Deploy

on:
  push:
    branches: [ "master" ]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v3

    - name: Login to DockerHub
      uses: docker/login-action@v3
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}

    - name: Build and Push to DockerHub
      uses: docker/build-push-action@v5
      with:
        context: .
        push: true
        tags: ${{ secrets.DOCKER_USERNAME }}/flask-app:latest

    - name: Deploy to EC2 via SSH
      uses: appleboy/ssh-action@v1.0.3
      with:
        host: ${{ secrets.EC2_HOST }}
        username: ${{ secrets.EC2_USER }}
        key: ${{ secrets.SSH_PRIVATE_KEY }}
        script: |
          docker pull ${{ secrets.DOCKER_USERNAME }}/flask-app:latest
          docker stop flask-app || true
          docker rm flask-app || true
          docker run -d -p 80:80 --name flask-app ${{ secrets.DOCKER_USERNAME }}/flask-app:latest
```

---

## Docker Support (Optional)

To build and run the app using Docker:

```bash
docker build -t flask-app .
docker run -p 5000:5000 flask-app
```

Then open `http://localhost:5000/` in your browser.

---
