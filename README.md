# DevOps Intern Final Assessment

**Name:** Kwame Aboagye Agyabeng
**Date:** August 11, 2026

## Project Overview

This project demonstrates a basic end-to-end DevOps workflow using:

* Linux
* Bash
* Git & GitHub
* Python
* Docker
* GitHub Actions
* Docker Hub
* HashiCorp Nomad
* Grafana Loki
* Grafana Alloy

The project demonstrates how source code moves through version control, automated testing, containerization, image publishing, deployment, and monitoring.

## Project Structure

```text
devops-intern-final/
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
├── monitoring/
│   └── loki_setup.txt
│
├── nomad/
│   └── hello.nomad
│
├── scripts/
│   └── linux_basics.sh
│
├── Dockerfile
├── hello.py
└── README.md
```

## 1. Linux and Bash

The `scripts/` directory contains Bash scripting exercises demonstrating basic Linux operations and shell scripting.

Example:

```bash
chmod +x scripts/linux_basics.sh
./scripts/linux_basics.sh
```

## 2. Python Application

The project contains a simple Python application in `hello.py`.

Run it locally with:

```bash
python hello.py
```

Expected output:

```text
Hello, DevOps!
```

## 3. Docker

The Python application is containerized using the `Dockerfile`.

Build the image locally:

```bash
docker build -t devops-hello:latest .
```

Run the container:

```bash
docker run --rm devops-hello:latest
```

Expected output:

```text
Hello, DevOps!
```

The Docker image is also published to Docker Hub as:

```text
devkwame/devops-hello:latest
```

## 4. GitHub Actions CI/CD

The GitHub Actions workflow is located at:

```text
.github/workflows/ci.yml
```

The workflow runs automatically when code is pushed to the repository.

The pipeline performs the following steps:

1. Checks out the repository.
2. Sets up Python 3.12.
3. Runs the Python application as a basic application check.
4. Logs in to Docker Hub using GitHub Actions Secrets.
5. Builds the Docker image.
6. Tags the image as `devkwame/devops-hello:latest`.
7. Pushes the Docker image to Docker Hub.

The Docker Hub credentials are stored securely as GitHub repository secrets:

```text
DOCKERHUB_USERNAME
DOCKERHUB_TOKEN
```

The Docker Hub token is not stored directly in the workflow file.

### CI/CD Workflow

```text
GitHub Push
     │
     ▼
GitHub Actions
     │
     ├── Checkout Repository
     │
     ├── Setup Python 3.12
     │
     ├── Run Python Application
     │
     ├── Login to Docker Hub
     │
     ├── Docker Build
     │
     └── Docker Push
             │
             ▼
   devkwame/devops-hello:latest
```

This creates the required connection between the source code repository, CI pipeline, and Docker image used by the deployment platform.

## 5. Docker Hub

Docker Hub is used as the container image registry.

The image published by GitHub Actions is:

```text
devkwame/devops-hello:latest
```

The image can be pulled manually with:

```bash
docker pull devkwame/devops-hello:latest
```

The image can then be run with:

```bash
docker run --rm devkwame/devops-hello:latest
```

Expected output:

```text
Hello, DevOps!
```

The Docker Hub image provides the container artifact that is consumed by the Nomad deployment.

## 6. HashiCorp Nomad

The Nomad job specification is located at:

```text
nomad/hello.nomad
```

The Nomad job uses the Docker driver and pulls the application image from Docker Hub.

The job references:

```text
devkwame/devops-hello:latest
```

The configuration uses `force_pull = true` so that Nomad pulls the image from Docker Hub rather than relying on an older locally cached version.

Run the Nomad job with:

```bash
nomad job run nomad/hello.nomad
```

Check the job status:

```bash
nomad job status hello-devops
```

Check allocations:

```bash
nomad job allocs hello-devops
```

The Nomad deployment is configured as a batch job because the Python application performs a short task and then exits successfully.

A successful allocation reports:

```text
Status = complete
```

### Deployment Flow

```text
Docker Hub
     │
     │ devkwame/devops-hello:latest
     ▼
   Nomad
     │
     │ force_pull = true
     ▼
Docker Container
     │
     ▼
Application Execution
```

## 7. Monitoring with Grafana Loki

Grafana Loki was configured locally using Docker.

A Docker network was created for the monitoring components:

```bash
docker network create monitoring
```

Loki was started with:

```bash
docker run -d \
  --name loki \
  --network monitoring \
  -p 3100:3100 \
  grafana/loki:latest
```

Verify that Loki is running:

```bash
docker ps
```

Check Loki readiness:

```bash
curl http://localhost:3100/ready
```

Expected response:

```text
ready
```

Docker container logs were forwarded to Loki using Grafana Alloy.

The Loki labels can be checked with:

```bash
curl "http://localhost:3100/loki/api/v1/labels"
```

The monitoring setup and commands are documented in:

```text
monitoring/loki_setup.txt
```

### Monitoring Flow

```text
Docker Container
       │
       │ Container Logs
       ▼
Grafana Alloy
       │
       │ Log Forwarding
       ▼
     Loki
       │
       ▼
 Log Storage
       │
       ▼
Log Monitoring
```

## 8. Git and GitHub

The project was managed using Git and GitHub.

Example commands used during development:

```bash
git status

git add .

git commit -m "ci: add Docker Hub image build and push"

git push origin main
```

The repository contains the source code, Docker configuration, CI/CD workflow, Nomad deployment configuration, monitoring configuration, and documentation.

## 9. End-to-End DevOps Workflow

The complete workflow is:

```text
Python Application
       │
       ▼
      Git
       │
       ▼
GitHub Repository
       │
       ▼
GitHub Actions
       │
       ├── Test Application
       │
       ├── Build Docker Image
       │
       └── Push Image
              │
              ▼
          Docker Hub
              │
              │ devkwame/devops-hello:latest
              ▼
            Nomad
              │
              │ force_pull = true
              ▼
        Docker Container
              │
              ▼
       Application Execution
              │
              ▼
       Grafana Alloy
              │
              ▼
             Loki
              │
              ▼
        Log Monitoring
```

This workflow demonstrates the integration between CI/CD, containerization, image management, deployment, and monitoring.

## 10. Key DevOps Concepts Demonstrated

### Version Control

Git was used to track changes and GitHub was used as the remote repository.

### Containerization

Docker packages the Python application and its runtime environment into a portable container image.

### Continuous Integration

GitHub Actions automatically checks the application when changes are pushed to the repository.

### Continuous Delivery

GitHub Actions builds the Docker image and publishes it to Docker Hub automatically after the application check succeeds.

### Container Registry

Docker Hub stores the published container image:

```text
devkwame/devops-hello:latest
```

### Orchestration

HashiCorp Nomad was used to schedule and run the Docker container.

### Monitoring

Grafana Loki was used as a centralized log storage system, while Grafana Alloy discovers and forwards Docker container logs to Loki.

## 11. Final Deliverables

The repository contains:

* `README.md` — Project documentation and run instructions
* `scripts/` — Linux/Bash exercises
* `Dockerfile` — Container configuration
* `.github/workflows/ci.yml` — GitHub Actions CI/CD pipeline
* `nomad/hello.nomad` — Nomad deployment configuration
* `monitoring/loki_setup.txt` — Loki and log-monitoring instructions
* `hello.py` — Python application

## 12. Assessment Improvement

Following the assessment feedback, the CI/CD workflow was updated to include the missing Docker image build and publishing steps.

The updated workflow now:

1. Runs the application check.
2. Authenticates securely with Docker Hub.
3. Builds the Docker image.
4. Tags the image as `devkwame/devops-hello:latest`.
5. Pushes the image to Docker Hub.
6. Provides the image consumed by the Nomad deployment.

This establishes a complete connection between GitHub Actions and the Docker image expected by the Nomad job.

## Conclusion

This assessment demonstrates an end-to-end DevOps workflow covering Linux scripting, version control, application execution, containerization, automated CI/CD, Docker image publishing, workload orchestration, centralized logging, monitoring, and technical documentation.

The final workflow connects GitHub Actions with Docker Hub and Nomad, while Grafana Alloy and Loki provide log collection and monitoring for the deployed container.
