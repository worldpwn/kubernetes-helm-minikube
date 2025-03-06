# Kubernetes Helm Minikube

## Build Status
[![Multi Service Deployment](https://github.com/worldpwn/kubernetes-helm-minikube/actions/workflows/multi-service-cd.yaml/badge.svg)](https://github.com/worldpwn/kubernetes-helm-minikube/actions/workflows/multi-service-cd.yaml) 
[![Build Service A Docker Image](https://github.com/worldpwn/kubernetes-helm-minikube/actions/workflows/service-a-build.yaml/badge.svg)](https://github.com/worldpwn/kubernetes-helm-minikube/actions/workflows/service-a-build.yaml)
[![Build Service B Docker Image](https://github.com/worldpwn/kubernetes-helm-minikube/actions/workflows/service-b-build.yaml/badge.svg)](https://github.com/worldpwn/kubernetes-helm-minikube/actions/workflows/service-b-build.yaml)

## Code Quality

[![CodeQL](https://github.com/worldpwn/kubernetes-helm-minikube/actions/workflows/github-code-scanning/codeql/badge.svg)](https://github.com/worldpwn/kubernetes-helm-minikube/actions/workflows/github-code-scanning/codeql)

## Deployment Status

[![Deploy Service A](https://github.com/worldpwn/kubernetes-helm-minikube/actions/workflows/service-a-cd.yaml/badge.svg)](https://github.com/worldpwn/kubernetes-helm-minikube/actions/workflows/service-a-cd.yaml)
[![Deploy Service B](https://github.com/worldpwn/kubernetes-helm-minikube/actions/workflows/service-b-cd.yaml/badge.svg)](https://github.com/worldpwn/kubernetes-helm-minikube/actions/workflows/service-b-cd.yaml)

## Linting

[![Lint Helm](https://github.com/worldpwn/kubernetes-helm-minikube/actions/workflows/lint-helm.yaml/badge.svg)](https://github.com/worldpwn/kubernetes-helm-minikube/actions/workflows/lint-helm.yaml)


# Summary

## Key Highlights of My Implementation

### Continuous Delivery & Shift-Left Approach
- Followed Continuous Delivery (CD) principles, ensuring automated and seamless deployments.
- Applied a Shift-Left approach by integrating early testing, security scanning, and quality checks into the pipeline.

### Code Quality & Testability Enhancements
- Refactored and improved code structure to enhance maintainability and enable effective testing.
- Applied Test-Driven Development (TDD) by writing tests to: Validate the health of each microservice and Ensure correct communication between Service A & Service B.

### Security & DevSecOps Practices
- Added static code analysis to enforce quality and security standards.
- Used GitHub Secrets to manage sensitive information securely, including Docker authentication.
- Protected the main branch by requiring checks before merging, ensuring only validated changes are deployed.

### CI/CD Pipeline & Automation
- Built an end-to-end CI/CD pipeline (GitHub Actions)
- Automate build, test, security scanning, and deployment.
- Ensure only validated code is merged and deployed.

## What I Didn’t Include (But Would in a Real-World Scenario)

### Security Hardening for Kubernetes
- Run pods as non-root users with minimal privileges.
- Implement network policies to follow Zero Trust principles.
- Use an external secret manager to store secrets and keys (Azure Key-Vault, AWS Secrets Manager etc)
- For Authentication and Authorisation will use RoleBase Access or IAM or any other Identity Provider to offload and federate this process. 
- Advanced Image Security & Supply Chain Integrity
- Perform image scanning to detect vulnerabilities before deployment.
- Use image signing to verify container integrity.

### Optimized CI/CD Workflow
- Improve monorepo setup to prevent unnecessary checks from running.

### High Availability & Resilience
- Implement multi-environment deployments (staging, pre-prod, production) for validation before release.
- Add rollback mechanisms & canary deployments for safe production releases.



# How to Build, Deploy, and Verify

### Build Your Deployment Artifacts

1. **Build Service A Docker Image:**
    ```sh
    docker build -t service-a:local ./applications/service-a/src
    ```

2. **Build Service B Docker Image:**
    ```sh
    docker build -t service-b:local ./applications/service-b/src
    ```

### Deploy Everything Using Helm

[Minikube installation](https://minikube.sigs.k8s.io/docs/start/?arch=%2Fmacos%2Farm64%2Fstable%2Fbinary+download)

1. **Start Minikube:**
    ```sh
    minikube start
    ```

2. **Deploy Service A:**
    ```sh
    helm install service-a applications/service-a/helm --create-namespace --namespace webapp
    ```

3. **Deploy Service B:**
    ```sh
    helm install service-b applications/service-b/helm --create-namespace --namespace webapp 
    ```

### Verify That Both Services Are Running and Communicating

1. **Check the status of the deployments:**
    ```sh
    kubectl rollout status deployment/service-a -n webapp
    kubectl rollout status deployment/service-b -n webapp
    ```

2. **Get the list of pods:**
    ```sh
    kubectl get pods -n webapp
    ```

3. **Run an ephemeral pod to test communication:**
    ```sh
    kubectl run test-pod -n webapp -it --image=curlimages/curl:latest -- /bin/sh 
    curl -s http://service-b:8012/ping_service_a
    ```

This will verify that

