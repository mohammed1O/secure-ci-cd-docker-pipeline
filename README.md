# secure-ci-cd-docker-pipeline

Automate the full lifecycle of Docker-based microservices
It handles:

Build & Push

Builds 6 Docker images from different app components

Pushes them to GitLab's container registry

Security Scanning

Scans each image for vulnerabilities using GitLab's built-in tools

Code Quality & Service Testing

Checks Python code style and complexity

Spins up services using Docker Compose

Verifies that endpoints respond correctly using curl

Pre-Production Deployment

Automatically updates a separate GitLab repo when a new commit or tag is pushed

Production Release (Manual)

 manually create a GitLab release tagged as production-ready


