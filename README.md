# Doc Pipeline

A lightweight CI/CD project that packages a static HTML website into a Docker container and deploys it through Jenkins.

## Overview

This repository contains a single-page HTML landing page served by Nginx inside a Docker container. The project demonstrates a simple deployment pipeline where code is checked out, a Docker image is built, pushed to Docker Hub, and then deployed as a running container.

### Included files
- `index.html` — the static web page
- `Dockerfile` — builds the Nginx container
- `jen_file` — Jenkins pipeline configuration
- `timer.html` — additional HTML page

## How it works

The Jenkins pipeline performs the following stages:

1. Git checkout
2. Build Docker image
3. Log in to Docker Hub
4. Push image to Docker Hub
5. Stop and remove any old container
6. Start the new container on port `4320`

This setup allows the project to be deployed automatically after updates.

## Docker configuration

The app uses the official `nginx:latest` image and serves the static site from `/usr/share/nginx/html/`:

```dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

## Run locally

Build the image:

```bash
docker build -t doc_pipeline:latest .
```

Run the container:

```bash
docker run -d --name doc_pipeline -p 4320:80 doc_pipeline:latest
```

Open in a browser:

```bash
http://localhost:4320
```

## Jenkins pipeline notes

The pipeline logs into Docker Hub using a Jenkins credential named `GOAT`. Make sure that credential exists in your Jenkins instance before running the pipeline.

## Purpose

This project is a basic example of:
- static site hosting
- Docker image creation
- Jenkins automation
- Docker Hub publishing
- container deployment

## License

This project is provided as-is for learning and demonstration purposes.
