Boardgame Application CI/CD Pipeline & Kubernetes Deployment
Overview
This project demonstrates a complete CI/CD pipeline using Jenkins to build, scan, and deploy a containerized application to a Kubernetes cluster on AWS EC2 instances. The pipeline includes code checkout, static code analysis, Docker image build, vulnerability scanning, image push to AWS ECR, and deployment to Kubernetes.

Table of Contents
Pipeline Steps
Project Structure
Jenkins Pipeline Details
Kubernetes Manifests
AWS ECR Setup
Kubernetes Setup
Prerequisites
How to Run the Pipeline
Accessing the Application
Troubleshooting
Pipeline Steps
checkout: Clone source code from GitHub
sonar: Static code analysis using SonarQube
quality-gate: Wait for SonarQube quality gate result
maven-build: Build application using Maven
ecr-login: Authenticate Docker to AWS ECR
docker-build: Build Docker image
trivy-scan: Scan Docker image for vulnerabilities using Trivy
docker-push: Push Docker image to AWS ECR
deploy: Deploy application to Kubernetes cluster
Project Structure
text

.
├── Dockerfile
├── Jenkinsfile
├── k8s
│   ├── deployment.yaml
│   └── service.yaml
├── README.md
└── (application source code)
Jenkins Pipeline Details
Jenkinsfile
The pipeline is defined in the Jenkinsfile and performs the following:

Checks out the code from GitHub
Runs SonarQube analysis for static code checks
Builds the application using Maven
Builds a Docker image and tags it with latest
Scans the Docker image using Trivy for vulnerabilities
Authenticates and pushes the image to AWS ECR
Deploys the application to a Kubernetes cluster using kubectl
Key Environment Variables
ECR_REPO: AWS ECR repository URI
AWS_REGION: AWS region for ECR
IMAGE_NAME: Full image name with tag
KUBECONFIG: Jenkins credential for Kubernetes access
Credentials Required in Jenkins
sonarqube-token: Secret text (SonarQube token)
aws-ecr-jenkins: Username/Password (AWS Access Key ID/Secret)
kube-creds: Secret file (Kubeconfig for cluster access)
