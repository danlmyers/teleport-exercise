---
authors: Daniel Myers
state: draft
---

# RFD 001 - Kubernetes Deployment Management Service

## What

The Kubernetes Deployment Management Service is an HTTP API designed for managing Kubernetes deployments.

## Why

This service is proposed to streamline Kubernetes deployment management, improve scalability and availability of applications, and enhance security measures in client-server communication.

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
- **Unit Tests:** To cover individual components.
- **Integration Tests:** To test the service with a Kubernetes cluster.
- **End-to-End Tests:** To validate the entire workflow of the service.

## Conclusion
The proposed service aims to offer a robust and secure solution for managing Kubernetes deployments, with an emphasis on simplicity, efficiency, and scalability.
