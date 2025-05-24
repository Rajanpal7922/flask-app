
# Flask App

A simple Flask web application with GitHub Actions CI/CD pipeline integration.

---

## Overview

This repository contains a minimal Flask application designed as a starting point for web development with Python. It includes:

- A simple route handler
- CI/CD integration using GitHub Actions
- Docker support for containerized deployment to AWS EC2

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
│       └── docker-push.yml   # CI/CD deployment pipeline configuration
└── Dockerfile          # Containerization setup
```

---

## Installation (Local)

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

## Deployment: CI/CD Pipeline with GitHub Actions

This project uses a GitHub Actions workflow to build, push, and deploy the Flask app to an AWS EC2 instance via Docker.

---

### Workflow File

Location: `.github/workflows/docker-push.yml`

---

### Deployment Workflow Summary

1. GitHub Actions is triggered on every push to the `master` branch.  
2. It builds a Docker image of the Flask app using `docker/build-push-action`.  
3. The image is pushed to DockerHub under your account.  
4. GitHub Actions then uses SSH to connect to your EC2 instance.  
5. It pulls the latest Docker image and runs it as a container on EC2.  

---

### GitHub Secrets Required

Make sure you define the following secrets in your GitHub repository under:  
**Settings → Secrets and Variables → Actions**

- `DOCKER_USERNAME`: Your DockerHub username  
- `DOCKER_PASSWORD`: Your DockerHub password or token  
- `EC2_HOST`: The public IP address of your EC2 instance  
- `EC2_USER`: The SSH username (e.g., `ec2-user` or `ubuntu`)  
- `SSH_PRIVATE_KEY`: Your EC2 private key used to SSH into the instance  

---

## Docker Support (Local)

To build and run the app locally using Docker:

```bash
docker build -t flask-app .
docker run -p 5000:5000 flask-app
```

Then open `http://localhost:5000/` in your browser.

---

## Deployed Version

When deployed via CI/CD, the Flask app runs on port 80 of your EC2 instance and is accessible using:

```
http://<your-ec2-public-ip>/
```

Replace `<your-ec2-public-ip>` with your actual EC2 IP address.
