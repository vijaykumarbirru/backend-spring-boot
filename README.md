# Spring Boot Hello World with GitHub Actions and AWS ECR

This project demonstrates:
- A simple Spring Boot Hello World application
- Docker image build and push to AWS ECR using GitHub Actions

## Steps
1. Configure GitHub Secrets:
   - `AWS_ACCESS_KEY_ID`
   - `AWS_SECRET_ACCESS_KEY`
   - `AWS_REGION`
   - `ECR_REPOSITORY`
   - `AWS_ACCOUNT_ID`

2. Workflow will:
   - Build Docker image
   - Authenticate to AWS ECR
   - Push image to ECR

## Build locally
```bash
mvn clean package
```

## Run locally
```bash
docker build -t springboot-hello .
docker run -p 8080:8080 springboot-hello
```


## Source Code Structure
- `src/main/java/com/example/demo/DemoApplication.java`: Main Spring Boot app with Hello World endpoint.
- `pom.xml`: Maven build file.

## Run locally
```bash
mvn spring-boot:run
```


## Deployment to AWS EKS
This workflow now:
- Builds and pushes Docker image to AWS ECR
- Configures AWS credentials using OIDC
- Updates kubeconfig for EKS cluster
- Deploys the app using Helm

### Required Additional Secrets
- `AWS_ROLE_TO_ASSUME`: IAM role for GitHub OIDC
- `EKS_CLUSTER_NAME`: Name of your EKS cluster

### Helm Chart
Place your Helm chart in `helm-chart/` directory inside the repo.


## Helm Deployment
To deploy the Spring Boot app to EKS using Helm:
```bash
helm upgrade --install springboot-app ./helm-chart   --namespace app --create-namespace   --set image.repository=<AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/<ECR_REPOSITORY>   --set image.tag=<IMAGE_TAG>
```
