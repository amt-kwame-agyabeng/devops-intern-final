# DevOps Intern Final Assessment

**Name:** Kwame Aboagye Agyabeng
**Date:** August 11, 2026

## Project Overview

This project demonstrates a basic DevOps workflow using:

* Linux
* Bash
* Git & GitHub
* Python
* Docker
* GitHub Actions
* HashiCorp Nomad
* Grafana Loki

The project demonstrates how source code can move through development, containerization, CI/CD, deployment, and monitoring.

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

Build the image:

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

## 4. GitHub Actions CI/CD

The GitHub Actions workflow is located at:

```text
.github/workflows/ci.yml
```

The workflow runs automatically when changes are pushed to the repository or when a pull request is created.

The pipeline:

1. Checks out the repository.
2. Sets up Python 3.12.
3. Runs the Python application.

Workflow file:

```text
.github/workflows/ci.yml
```

## 5. HashiCorp Nomad

The Nomad job specification is located at:

```text
nomad/hello.nomad
```

The job uses the Docker driver and pulls the container image from Docker Hub.

Run the Nomad job with:

```bash
nomad job run nomad/hello.nomad
```

Check the job status:

```bash
nomad job status hello-devops
```

Check an allocation:

```bash
nomad job allocs hello-devops
```

The Nomad deployment was configured as a batch job because the Python application performs a short task and then exits successfully.

A successful allocation reports:

```text
Status = complete
```

## 6. Monitoring with Grafana Loki

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

## 7. Git and GitHub

The project was managed using Git.

Example commands used during development:

```bash
git status
git add .
git commit -m "Complete DevOps intern final assessment"
git push origin main
```

The repository contains the source code, Docker configuration, CI workflow, Nomad deployment configuration, monitoring configuration, and documentation.

## 8. End-to-End Workflow

The project demonstrates the following workflow:

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
       ▼
    Docker
       │
       ▼
 Docker Hub
       │
       ▼
    Nomad
       │
       ▼
 Container Execution
       │
       ▼
Grafana Alloy → Loki
       │
       ▼
   Log Monitoring
```

## 9. Key DevOps Concepts Demonstrated

### Version Control

Git was used to track changes and GitHub was used as the remote repository.

### Containerization

Docker packages the Python application and its runtime environment into a portable container image.

### CI/CD

GitHub Actions automatically checks the application when changes are pushed or submitted through a pull request.

### Orchestration

HashiCorp Nomad was used to schedule and run the Docker container.

### Monitoring

Grafana Loki was used as a centralized log storage system, with Grafana Alloy forwarding Docker container logs to Loki.

## 10. Final Deliverables

The repository contains:

* `README.md` — Project documentation and run instructions
* `scripts/` — Linux/Bash exercises
* `Dockerfile` — Container configuration
* `.github/workflows/ci.yml` — GitHub Actions CI pipeline
* `nomad/hello.nomad` — Nomad deployment configuration
* `monitoring/loki_setup.txt` — Loki setup and log-monitoring instructions
* `hello.py` — Python application

## Conclusion

This assessment demonstrates a basic end-to-end DevOps workflow covering Linux scripting, version control, containerization, CI/CD, workload orchestration, monitoring, and technical documentation.
