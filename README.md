## Swiggy Clone Frontend
[](https://github.com/eissa/swiggy-project/actions)
[](Dockerfile)
[](LICENSE)

<img width="1920" height="1080" alt="Screenshot from 2026-09-25 14-24-49" src="https://github.com/user-attachments/assets/5e3c36d1-4e6e-4cec-81bf-a58d0bd74a82" />

<img width="1920" height="1080" alt="Screenshot from 2026-09-25 14-25-23" src="https://github.com/user-attachments/assets/a33efd50-31c9-427b-bc82-056ec9509ba5" />

<img width="1920" height="264" alt="Screenshot from 2026-09-25 14-25-43" src="https://github.com/user-attachments/assets/c458d62d-9d84-4a00-a41b-aa9060a5e50d" />

A high-fidelity Swiggy Clone Frontend web application built with React, configured for optimized static distribution, and fully automated via an on-premise CI/CD pipeline. The project features an automated containerized workflow that builds and serves production assets via a lightweight Nginx server upon every code push [image_UhS79i.png, image_kbOXLh.png].
------------------------------
## Key Features

* Responsive Food Delivery UI: Implements a clean, multi-banner food discovery layout mimicking production marketplaces, featuring responsive restaurant grids, carousel promotions, and localized search mockups [image_kbOXLh.png].
* Multi-Stage Containerization: Utilizes an optimized two-stage Dockerfile to dramatically minimize final image sizes by decoupling the Node.js compilation environment from the runtime layer [image_UhS79i.png].
* On-Premise Continuous Deployment: Integrates a self-hosted GitHub Actions workflow for zero-downtime micro-deployments directly to target host environments on branch updates [image_MVi8va.png].

------------------------------
## Tech Stack

* Frontend Architecture: [Frontend Tech Stack Placeholder]
* Production Web Server: Nginx Alpine (Lightweight static asset server)
* Container Orchestration: Docker, Docker Compose (v3.8) [image_UhS79i.png]
* Automation Suite: GitHub Actions (Self-Hosted Runner Integration) [image_MVi8va.png]

------------------------------
## System & Deployment Architecture
The application handles modifications on the main branch through a GitOps-driven deployment paradigm:

[ Developer Push ] ──> [ GitHub Repository ]
                               │ (Triggers Workflow)
                               ▼
                    [ Self-Hosted Runner ]
                               │ 
                               ▼ (Executes Automation Tasks)
                    [ Docker Compose Runtime ]
            ┌──────────────────┴──────────────────┐
            ▼                                     ▼
   (Stage 1: Build Layer)               (Stage 2: Web Server)
    node:20-alpine                        nginx:alpine
  [npm ci -> npm run build]           [Serves /usr/share/nginx/html]
                                                  │
                                                  ▼
                                         [ http://localhost:80 ]

------------------------------
## Getting Started## Prerequisites
Ensure the following runtimes are installed on your host system or deployment target:

* Docker Engine (>= 24.0.0)
* Docker Compose (>= 2.20.0)
* GitHub Actions Self-Hosted Runner configured and operational on the target system [image_MVi8va.png].

## Configuration Files
The repository ships pre-configured with production infrastructure matrices:
## 1. Multi-Stage Dockerfile

FROM node:20-alpine AS buildWORKDIR /appCOPY package*.json ./RUN npm ciCOPY . .RUN npm run build
FROM nginx:alpineCOPY --from=build /app/build /usr/share/nginx/htmlEXPOSE 80CMD ["nginx", "-g", "daemon off;"]

## 2. Orchestration Layer (docker-compose.yml)

version: '3.8'
services:
  frontend:
    container_name: swiggy-frontend
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80:80"
    restart: always

------------------------------
## CI/CD Automation Pipeline
Automated workflows are managed under .github/workflows/deploy.yml. The job executes targeting local platform infrastructure using a self-hosted compute loop [image_MVi8va.png].

name: CI/CD Deploy to Local Laptop
on:
  push:
    branches:
      - main
jobs:
  deploy:
    runs-on: self-hosted

    steps:
      - name: Checkout Repository Code
        uses: actions/checkout@v4

      - name: Build and Run Docker Compose
        run: |
          docker compose down
          docker compose up --build -d

## Local Manual Pipeline Emulation
To trigger the build orchestration chain manually on your workstation without committing code, execute:

# Tear down existing assets and clear orphaned network segments
docker compose down
# Force a clean multi-stage compilation and isolate the process daemon
docker compose up --build -d

------------------------------
## Verification & Verification Signatures
Once the pipeline completes execution, you can verify container state and networking using native command interfaces:
## 1. Monitor Active Microservices

docker ps

Expected Terminal Output Signature: [image_UhS79i.png]

CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS         PORTS                               NAMES
f323b5286ed2   swiggy-frontend   "/docker-entrypoint…"   27 seconds ago  Up 27 seconds  0.0.0.0:80->80/tcp, [::]:80->80/tcp  swiggy-frontend

## 2. Accessing the UI Layer
Open your preferred web browser interface and navigate to the loopback networking address [image_kbOXLh.png]:

http://localhost:80

------------------------------
## Contributing

   1. Create a descriptive feature branch (git checkout -b feature/amazing-ui).
   2. Commit updates adhering to standardized message structures (git commit -m 'feat: add local navigation layout').
   3. Push changes to the repository upstream remote.
   4. Issue a formal Pull Request against the target stabilization branch.

------------------------------
## License
Distributed under the MIT License. See LICENSE for more details.
------------------------------
