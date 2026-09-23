	# Doc Pipeline

A lightweight CI/CD project that packages a static HTML website into a Docker container and deploys it through Jenkins.

## Overview

This repository contains a single-page HTML landing page served by Nginx inside a Docker container. The project is designed to demonstrate a simple deployment pipeline where code is checked out, a Docker image is built, pushed to Docker Hub, and then deployed as a running container.

The repository includes:
- `index.html` — the web page served by the container
- `Dockerfile` — builds the Nginx-based container image
- `jen_file` — Jenkins pipeline configuration
- `timer.html` — an additional page/template

## Architecture

The CI/CD flow is defined in the Jenkins pipeline and includes the following stages:

1. Git checkout
2. Build Docker image
3. Docker Hub login
4. Push image to Docker Hub
5. Stop and remove any existing container
6. Run the updated container on port `4320`

This allows the site to be deployed automatically after code changes.

## Docker Setup

The container uses the official `nginx:latest` base image and copies the static site into the default Nginx HTML directory:

```dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

'''

## Run locally

Build the image:
docker build -t doc_pipeline:latest .
