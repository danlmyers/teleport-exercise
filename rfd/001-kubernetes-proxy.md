---
authors: Daniel Myers
state: draft
---

# RFD 001 - Kubernetes Deployment Management Service

## What

The Kubernetes Deployment Management Service is an HTTP API designed for managing Kubernetes deployments.

## Why

This service is developed as a part of the Level 4 exercise for Teleport's Site Reliability Engineering (SRE) role. It aims to streamline Kubernetes deployment management, demonstrating capabilities in handling Kubernetes resources effectively.

## Details

### Endpoints
1. **Get Replica Count**
    - `GET /deployments/{name}/replicas`
    - **Example Output:**
      ```json
      {
        "deploymentName": "nginx-deployment",
        "replicaCount": 3
      }
      ```

2. **Set Replica Count**
    - `POST /deployments/{name}/replicas`
    - **Example Output:**
      ```json
      {
        "deploymentName": "nginx-deployment",
        "updatedReplicaCount": 5
      }
      ```

3. **List Deployments**
    - `GET /deployments`
    - **Example Output:**
      ```json
      [
        {
          "deploymentName": "nginx-deployment",
          "replicaCount": 3
        },
        {
          "deploymentName": "redis-deployment",
          "replicaCount": 1
        }
      ]
      ```

4. **Health Check**
    - `GET /health`
    - **Example Output:**
      ```json
      {
        "status": "healthy",
        "kubernetes": "connected"
      }
      ```

### Security
- **mTLS with Self-Signed Certificates:**
    - Implement mutual TLS (mTLS) using self-signed certificates for secure communication.
    - Pre-generate the keys and certificates and store them securely in a designated folder alongside the service code.
    - Configure the HTTP server to utilize these certificates and keys for establishing secure connections.
    - Ensure clients have access to their respective certificates and private keys for authentication.


### Caching
- **Replica Count Caching:** Implement caching to store replica counts of Kubernetes deployments. This involves:
   - Setting up a watch on Kubernetes Deployment resources to receive updates.
   - Storing replica counts in a local cache, updated in real-time from the watch.
   - Serving replica count requests from the cache rather than making frequent API calls to Kubernetes.

### Automation & Deployment
- **Dockerization:** Create a Dockerfile for building the service into a container.
- **Helm Chart:** Develop a Helm chart for easy deployment into Kubernetes environments.

### Test Plan

- **Unit Tests:**
    - Test individual components such as `DeploymentCache` methods, ensuring thread-safe operations.
    - Validate utility functions like Kubernetes client configuration.
    - Mock Kubernetes client for testing handlers without actual Kubernetes interaction.

- **Integration Tests:**
    - Deploy the service in a controlled Kubernetes environment.
    - Verify the service's interaction with Kubernetes API, particularly the ability to watch for deployment changes.
    - Test API endpoints against the actual Kubernetes cluster to ensure correct behavior.

- **End-to-End Tests:**
    - Simulate real-world usage scenarios, including updating deployments.
    - Verify that API endpoints return correct data and handle edge cases gracefully.



## Conclusion
The proposed service aims to offer a robust and secure solution for managing Kubernetes deployments, with an emphasis on simplicity, efficiency, and scalability.
