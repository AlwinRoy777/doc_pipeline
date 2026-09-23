# 🚀 Dockerized Static Website CI/CD Pipeline — Portfolio Project

> A lightweight DevOps project that packages a single-page HTML experience with **Docker**, publishes the image through **Docker Hub**, and automates deployment with a **Jenkins pipeline**.

[![HTML](https://img.shields.io/badge/HTML5-static_site-E34F26?logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML) [![Docker](https://img.shields.io/badge/Docker-containerized-2496ED?logo=docker&logoColor=white)](https://www.docker.com/) [![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-D24939?logo=jenkins&logoColor=white)](https://www.jenkins.io/) [![Nginx](https://img.shields.io/badge/Nginx-web_server-009639?logo=nginx&logoColor=white)](https://nginx.org/)

---

## 📌 Project Overview

**Doc Pipeline** demonstrates a simple, repeatable delivery workflow for a static website. The application is a single-page HTML electronic press kit experience served by Nginx inside a Docker container.

The Jenkins pipeline automates the main delivery stages:

- Checks out the `main` branch from GitHub
- Builds a Docker image from the repository
- Authenticates to Docker Hub through a Jenkins credential
- Pushes the image to Docker Hub
- Stops and removes the previous container when present
- Starts the new container and exposes it on host port `4320`

This project focuses on practical CI/CD fundamentals: source checkout, container image creation, registry publishing, and automated container replacement.

---

## 🏗️ Architecture & Workflow

```text
┌─────────────────────┐
│  GitHub Repository  │
│  index.html         │
│  Dockerfile         │
│  jen_file           │
└──────────┬──────────┘
           │ Jenkins Git checkout
           ▼
┌─────────────────────┐
│   Jenkins Pipeline  │
│  Build → Login →    │
│  Push → Redeploy     │
└──────────┬──────────┘
           │ docker build / docker push
           ▼
┌─────────────────────┐
│      Docker Hub     │
│ bigchill10/nginx:latest │
└──────────┬──────────┘
           │ pull and run
           ▼
┌─────────────────────┐
│   Docker Container  │
│       Nginx         │
│  container port 80  │
└──────────┬──────────┘
           │ port mapping
           ▼
┌─────────────────────┐
│   Browser / Client  │
│  http://localhost:4320 │
└─────────────────────┘
```

---

## 🛠️ Technologies & Services Used

| Technology / Service | Purpose |
|---|---|
| **HTML5** | Provides the single-page static website experience |
| **Nginx** | Serves `index.html` from inside the container |
| **Docker** | Packages the website and web server into a portable image |
| **Docker Hub** | Stores and distributes the published container image |
| **Jenkins** | Automates checkout, image build, registry publishing, and redeployment |
| **GitHub** | Hosts the source code used by the pipeline |
| **Vanilla JavaScript / CSS** | Powers the interactive presentation and visual styling embedded in `index.html` |

---

## 📁 Repository Structure

```text
doc_pipeline/
│
├── README.md       # Project documentation
├── Dockerfile      # Builds the Nginx-based container image
├── jen_file        # Jenkins declarative pipeline configuration
├── index.html      # Single-page static website
└── timer.html      # Redirect page to TimerMo.com
```

---

## 🚀 Run Locally with Docker

### Prerequisites

- Docker installed and running
- Git, if cloning the repository locally

### 1. Clone the repository

```bash
git clone https://github.com/AlwinRoy777/doc_pipeline.git
cd doc_pipeline
```

### 2. Build the Docker image

```bash
docker build -t doc_pipeline:latest .
```

The `Dockerfile` uses `nginx:latest`, copies `index.html` into Nginx's document root, and exposes port `80` inside the container.

### 3. Start the container

```bash
docker run -d --name doc_pipeline -p 4320:80 doc_pipeline:latest
```

### 4. Open the site

Visit:

```text
http://localhost:4320
```

### 5. Stop and remove the container

```bash
docker stop doc_pipeline
docker rm doc_pipeline
```

---

## 🔁 Jenkins Pipeline Deployment

The pipeline definition is stored in [`jen_file`](./jen_file). It contains these stages:

1. **Git checkout** — retrieves the `main` branch from this repository.
2. **Build Docker image** — creates `bigchill10/nginx:latest`.
3. **Docker login** — authenticates with Docker Hub using the Jenkins credential ID `GOAT`.
4. **Push image** — publishes `bigchill10/nginx:latest`.
5. **Container creation and stopping old** — replaces the container named `gayle` and maps host port `4320` to container port `80`.

> **Configuration note:** Before running the pipeline, create a Jenkins username/password credential with the ID `GOAT`. The credential is consumed through Jenkins' `withCredentials` step rather than hard-coding a password in the pipeline.

To use the pipeline:

1. Create a Jenkins Pipeline job.
2. Point it to this repository and the `jen_file` pipeline script.
3. Ensure the Jenkins agent can run Docker commands.
4. Add the required Docker Hub credential with ID `GOAT`.
5. Run the job and verify the container on port `4320`.

---

## 🌐 Live Demo

No public deployment URL is defined in this repository. The documented runtime target is the local container endpoint:

```text
http://localhost:4320
```

---

## 📸 Screenshots

No screenshots are currently included in the repository. The local result can be viewed after running the Docker command above.

---

## 💡 Key Concepts Learned

- **Containerized static hosting** — Serving a static website through Nginx in a portable Docker image.
- **Docker image construction** — Using a minimal Dockerfile to copy application content into an existing web-server image.
- **CI/CD pipeline design** — Breaking delivery into clear checkout, build, authentication, push, and deployment stages.
- **Container replacement** — Stopping and removing an older container before starting the newly published version.
- **Registry publishing** — Moving a locally built image into Docker Hub for distribution.
- **Jenkins credentials** — Injecting registry credentials securely at runtime with `withCredentials`.
- **Port mapping** — Exposing Nginx's container port `80` through host port `4320`.

---

## ⚠️ Operational Notes

| Area | Implementation in this project |
|---|---|
| Registry authentication | Jenkins uses the credential ID `GOAT` and passes the password through `docker login --password-stdin`. |
| Repeated deployments | `docker container stop ... || true` and `docker container rm ... || true` allow replacement even when the previous container is absent. |
| Runtime access | Nginx listens on container port `80`; the deployment maps it to host port `4320`. |
| Image consistency | The Jenkins pipeline currently builds and pushes the `latest` tag. |

---

## 🔮 Future Improvements

- [ ] Replace the mutable `latest` tag with versioned image tags based on the Git commit SHA.
- [ ] Add a Docker health check and Jenkins post-build verification.
- [ ] Add a `docker-compose.yml` or equivalent deployment definition for repeatable local execution.
- [ ] Add a reverse proxy or HTTPS-enabled public deployment.
- [ ] Add automated tests or HTML validation before publishing the image.
- [ ] Add Jenkins build notifications and deployment rollback handling.
- [ ] Add repository screenshots or a live demo URL once a public deployment is configured.

---

## 📞 Connect

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://linkedin.com/in/alwinroy) [![GitHub](https://img.shields.io/badge/GitHub-AlwinRoy777-black?logo=github)](https://github.com/AlwinRoy777) [![Portfolio](https://img.shields.io/badge/Portfolio-Visit-cyan)](https://AlwinRoy777.github.io/Alwin-Portfolio/)

---

*Built as a practical CI/CD learning project by [Alwin Roy](https://github.com/AlwinRoy777).* 
