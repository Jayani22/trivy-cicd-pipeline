# GitHub Actions CI/CD Pipeline with Trivy Security Scan

## Project Overview

This project demonstrates a complete **CI/CD pipeline using GitHub Actions** with **container security scanning using Trivy** and **automatic deployment to an AWS EC2 instance**.

The pipeline builds a Docker image for a simple web application, scans it for vulnerabilities, pushes it to Docker Hub if the scan passes, and automatically redeploys the updated container on an EC2 server.

---

## Technologies Used

* GitHub Actions (CI/CD)
* Docker
* Trivy (Container Security Scanner)
* AWS EC2
* Docker Hub
* Nginx
* Linux (Ubuntu)

---

## CI Pipeline (Continuous Integration)

The CI workflow performs the following steps:

1. Triggered on every push to the `main` branch
2. Checks out the repository
3. Builds the Docker image
4. Runs a **Trivy vulnerability scan**
5. Fails the pipeline if **HIGH or CRITICAL vulnerabilities** are found

### Trivy Security Scan

Trivy scans the Docker image to detect:

* OS package vulnerabilities
* Library vulnerabilities
* Security misconfigurations

If HIGH or CRITICAL vulnerabilities are detected, the pipeline stops.

---

## CD Pipeline (Continuous Deployment)

The CD workflow runs only if the CI pipeline succeeds.

Steps:

1. Login to Docker Hub
2. Tag the Docker image using the **GitHub commit SHA**
3. Push the Docker image to Docker Hub
4. Connect to the EC2 instance via SSH
5. Pull the latest Docker image
6. Stop the existing container
7. Remove the old container
8. Start a new container with the updated image

---

## Docker Image Tagging

Each build uses a **unique tag** generated from the GitHub commit SHA.

Example:

```
devops-trivy-app:3c019d4
```

This ensures every deployment uses a unique version of the application.

---

## Deployment

The application is deployed on an **AWS EC2 instance** using Docker.

The container runs on **port 80** and serves the web application using **Nginx**.

---

## Access the Application

Once deployment completes, the application can be accessed using the EC2 public IP:

```
http://EC2_PUBLIC_IP
```

---

## Security Implementation

Security is enforced in the pipeline using **Trivy vulnerability scanning**.

Key features:

* Detects vulnerabilities in Docker images
* Blocks deployment if HIGH or CRITICAL vulnerabilities exist
* Ensures only secure images are deployed to production

---

## GitHub Secrets Used

The following secrets are configured in the GitHub repository:

* `DOCKER_USERNAME`
* `DOCKER_PASSWORD`
* `EC2_HOST`
* `EC2_USER`
* `EC2_SSH_KEY`

These secrets allow secure authentication for Docker Hub and the EC2 instance.

---