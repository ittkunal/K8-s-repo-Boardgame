# Boardgame Application CI/CD Pipeline & Kubernetes Deployment

## Overview

This project demonstrates a complete CI/CD pipeline using **Jenkins** to build, scan, and deploy a containerized application to a **Kubernetes cluster** on AWS EC2 instances. The pipeline includes code checkout, static code analysis, Docker image build, vulnerability scanning, image push to AWS ECR, and deployment to Kubernetes.

---

## Table of Contents

- [Pipeline Steps](#pipeline-steps)
- [Project Structure](#project-structure)
- [Jenkins Pipeline Details](#jenkins-pipeline-details)
- [Kubernetes Manifests](#kubernetes-manifests)
- [AWS ECR Setup](#aws-ecr-setup)
- [Kubernetes Setup](#kubernetes-setup)
- [Prerequisites](#prerequisites)
- [How to Run the Pipeline](#how-to-run-the-pipeline)
- [Accessing the Application](#accessing-the-application)
- [Troubleshooting](#troubleshooting)

---

## Pipeline Steps

1. **checkout**: Clone source code from GitHub
2. **sonar**: Static code analysis using SonarQube
3. **quality-gate**: Wait for SonarQube quality gate result
4. **maven-build**: Build application using Maven
5. **ecr-login**: Authenticate Docker to AWS ECR
6. **docker-build**: Build Docker image
7. **trivy-scan**: Scan Docker image for vulnerabilities using Trivy
8. **docker-push**: Push Docker image to AWS ECR
9. **deploy**: Deploy application to Kubernetes cluster

---

## Project Structure
