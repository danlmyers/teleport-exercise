# teleport-exercise
This is the repo for the Level 4 SRE challenge for Teleport.

https://github.com/gravitational/careers/blob/main/challenges/sre/challenge.md

## Level 4

### Server

* HTTP API to retrieve the replica count of the Kubernetes Deployment
* HTTP API to set the replica count of the Kubernetes Deployment
* HTTP API to get the list of available Deployments in the Kubernetes cluster
* HTTP health check verifying Kubernetes connectivity
* HTTP API must cache the replica count by watching for changes to Deployments.
  Read-only requests should not each trigger a request to the cluster.
  It is acceptable to use either client-go or controller-runtime to implement this.
* Secure connections between the HTTP API and caller with mTLS
* One or two tests that cover happy and unhappy scenarios

### Automation

* Ability to build the server in a Docker container
* Ability to run the server (inside and outside Kubernetes)
* Ability to execute integration tests against the local Kubernetes cluster
* Produce production ready releases (binaries and docker image)

### Deployment

* Create a helm chart for the service that includes at least: a Deployment, ServiceAccount and Service
* Deploy releases of the API Server to a Kubernetes cluster along with any dependencies

---
## Generate self signed certificates
```
openssl req -newkey rsa:2048 \
  -new -nodes -x509 \
  -days 3650 \
  -out cert.pem \
  -keyout key.pem \
  -subj "/C=US/ST=Utah/L=SLC/O=Teleport/OU=Engineering/CN=localhost"
```
