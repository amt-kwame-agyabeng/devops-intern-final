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
├── README.md
├── Dockerfile
│
├── scripts/
│   └── Linux/Bash scripts
│
├── nomad/
│   └── hello.nomad
│
├── monitoring/
│   ├── alloy-config.alloy
│   └── loki_setup.txt
│
└── .github/
    └── workflows/
        └── ci.yml
```

## 1. Linux Basics

The `scripts/` directory contains Bash scripts demonstrating basic Linux administration and command-line operations.

Scripts can be executed using:

```bash
chmod +x scripts/*.sh
./scripts/<script-name>.sh
```

## 2. Docker

The application is containerized using the `Dockerfile`.

Build the image:

```bash
docker build -t devkwame/devops-hello:latest .
```

Run the container:

```bash
docker run --rm devkwame/devops-hello:latest
```

Expected output:

```text
Hello, DevOps!
```

The image is also available from Docker Hub and is used by the Nomad deployment.

## 3. CI/CD

GitHub Actions is configured in:

```text
.github/workflows/ci.yml
```

The pipeline automatically checks the project and builds/tests the Docker image when changes are pushed to GitHub.

## 4. Nomad Deployment

The Nomad job definition is located at:

```text
nomad/hello.nomad
```

Run the job with:

```bash
nomad job run nomad/hello.nomad
```

Check the job status:

```bash
nomad job status hello-devops
```

The job is configured as a `batch` job because the application prints a message and exits successfully.

A successful allocation should show the task as `complete`.

## 5. Monitoring with Grafana Loki

Loki is used for centralized log collection and storage.

Start Loki:

```bash
MSYS_NO_PATHCONV=1 docker run -d \
  --name loki \
  --network monitoring \
  -p 3100:3100 \
  grafana/loki:latest \
  -config.file=/etc/loki/local-config.yaml
```

Verify Loki:

```bash
curl http://localhost:3100/ready
```

Expected output:

```text
ready
```

Grafana Alloy is used to collect Docker container logs and forward them to Loki.

The Alloy configuration is located at:

```text
monitoring/alloy-config.alloy
```

Detailed Loki setup instructions are available in:

```text
monitoring/loki_setup.txt
```

## 6. Verification

The main components can be verified with:

```bash
docker images
docker ps
nomad job status hello-devops
curl http://localhost:3100/ready
```

## Technologies Used

| Technology      | Purpose                           |
| --------------- | --------------------------------- |
| Linux/Bash      | System and scripting fundamentals |
| Git/GitHub      | Version control                   |
| Python          | Application used in the container |
| Docker          | Application containerization      |
| GitHub Actions  | CI/CD automation                  |
| HashiCorp Nomad | Workload deployment               |
| Grafana Loki    | Log aggregation and monitoring    |

## Conclusion

This project demonstrates a simple end-to-end DevOps workflow covering Linux scripting, version control, containerization, CI/CD, workload deployment, and centralized logging.
