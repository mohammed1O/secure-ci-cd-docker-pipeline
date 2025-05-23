# secure-ci-cd-docker-pipeline

This GitLab CI/CD pipeline automates the entire DevOps lifecycle for your Docker-based application. It includes:

Stage	What it does
build_push	Builds Docker images and pushes them to the GitLab Container Registry
vulnerability_scan	Scans images for security vulnerabilities
test	Performs code style/complexity checks and tests service readiness
pre_deploy	Deploys to a pre-production GitLab repo when pushed to main or tagged
prod_deploy	Manual trigger to release to production via GitLab Releases API

 Detailed Breakdown
🔹 1. build_push Stage
Builds 6 Docker images:

app_core

app_apache

nginx_reverse

app_redis_srv1

basic_web

basic_nfs

Pushes all images to the GitLab Container Registry using your project’s credentials.

🛡 2. vulnerability_scan Stage
Each image is scanned for known vulnerabilities using the gtcs scan tool (GitLab container scanning):

Reports are saved as gl-container-scanning-report.json.

These are retained for 5 months.

This is done for each Docker image separately.


 3. test Stage
This stage uses a Python 3.11 container to do several things:

Code style check using flake8 → reports/flake8-report.txt

Code complexity analysis using radon → reports/radon-report.txt

Service readiness tests:

Runs the stack with docker-compose up.

Uses curl (inside containers) to test HTTP endpoints.

If services don't respond within 30s, it exits with failure.

Artifacts (test reports) are stored for 5 months.

 4. pre_deploy Stage
Automatically runs on main, master, or tags.

Clones another GitLab repo: preprod-group-h

Creates/updates a file preprod-deployment.txt with the tag name

Commits and pushes it to the preprod repo.

 5. prod_deploy Stage
This stage is manual only (you must trigger it via GitLab UI).

Uses curl and the GitLab API to create a release for the tag.

Requires a Personal Access Token (glpat-...) to authenticate with GitLa
