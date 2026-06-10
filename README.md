# security-ci-cd-demo

Security CI/CD Pipeline Demonstration
A DevSecOps pilot project showcasing automated security validation using Docker, Trivy, and GitHub Actions.

1. Introduction
    This repository provides a reference implementation of a security‑focused CI/CD pipeline.
    The objective is to demonstrate how security scanning can prevent vulnerable container images from being built or deployed, while allowing secure images to pass through the pipeline successfully.
    
    The project includes:
        A simple application packaged into a Docker image
        A vulnerable and a secure Dockerfile
        A GitHub Actions workflow integrating Trivy vulnerability scanning

2. Architecture Overview
    The pipeline follows a standard DevSecOps workflow:

    Build a Docker image from source
    Scan the image using Trivy
    Fail the pipeline if HIGH or CRITICAL vulnerabilities are detected
    Pass the pipeline when a secure image is used

    => This approach ensures that insecure artifacts never progress beyond the build stage.

3. Repository Structure

    security-ci-cd-demo/
    │
    ├── app/
    │   └── main.py
    │
    ├── docker/
    │   ├── Dockerfile.vulnerable
    │   └── Dockerfile.secure
    │
    ├── trivy/
    │   ├── policy.rego
    │   └── allowlist.yaml
    │
    └── .github/
        └── workflows/
            └── ci.yml

4. Application Component
    The application is intentionally simple and serves only as a containerized workload for demonstrating the security pipeline.

    app/main.py
      python

      print("Hello World!")

5. Container Images
    5.1 Vulnerable Image
        The vulnerable Dockerfile uses an end‑of‑life Python base image to ensure that Trivy detects multiple high‑severity vulnerabilities.
        
		 docker/Dockerfile.vulnerable
        
         FROM python:3.12-slim
         WORKDIR /app
         COPY app/main.py .
         CMD ["python", "main.py"]

    5.2 Secure Image
        The secure Dockerfile uses a modern, maintained base image with significantly fewer vulnerabilities.
        
         docker/Dockerfile.secure
             
         FROM python:3.12-alpine
         WORKDIR /app
         COPY app/main.py .
         CMD ["python", "main.py"]

6. CI/CD Pipeline
    The GitHub Actions workflow performs the following steps:

    Checks out the repository
    Builds the Docker image
    Executes a Trivy vulnerability scan
    Fails the build if HIGH or CRITICAL vulnerabilities are detected

    The workflow file is located at: .github/workflows/ci.yml

7. Testing the Pipeline
    7.1 Failure Scenario
        To demonstrate security enforcement:
           7.1.1. Configure the workflow to use Dockerfile.vulnerable
           7.1.2. Commit and push to the repository
           7.1.3. The pipeline will fail due to detected vulnerabilities
    7.2 Success Scenario
        To demonstrate a secure build:
           7.2.1. Update the workflow to use Dockerfile.secure
           7.2.2. Commit and push
           7.2.3. The pipeline will complete successfully

8. Repository Access
    The project can be shared using the standard GitHub URL format: https://github.com/security-sample/security-ci-cd-demo
