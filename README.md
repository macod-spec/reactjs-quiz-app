
React Quiz App – Cloud-Native Full Stack Application (Docker + Kubernetes + GCP)

This project is a full-stack quiz application built with React, Node.js, and MongoDB, deployed using Docker, Kubernetes (GKE), and Google Cloud Platform.


My Contribution

I extended this project beyond a basic setup by:

* Containerizing the frontend and backend using Docker
* Building and pushing images to Google Artifact Registry
* Deploying the application to Google Kubernetes Engine (GKE)
* Creating Kubernetes manifests for:
    * Deployments
    * Services
    * Namespace isolation
* Configuring internal service communication within Kubernetes
* Exposing only the frontend via a public LoadBalancer
* Securing the backend by restricting it to internal cluster access
* Seeding MongoDB from within the Kubernetes cluster
* Troubleshooting networking issues between frontend and backend

Note: This repository was originally forked. My work focuses on containerization, cloud deployment, and Kubernetes orchestration.



Architecture

* Frontend: React (served via Kubernetes LoadBalancer on port 8080)
* Backend: Node.js API (internal Kubernetes service on port 3000)
* Database: MongoDB (internal service on port 27017)
* Containerization: Docker
* Orchestration: Kubernetes (GKE)
* Cloud Platform: Google Cloud Platform (GCP)



System Flow

User → Frontend (LoadBalancer) → Backend (ClusterIP) → MongoDB

* Only the frontend is publicly accessible
* Backend and database are internal to the cluster



Deployment (Kubernetes on GCP)

1. Build and push images

gcloud builds submit ./quiz-app \
--tag europe-west2-docker.pkg.dev/<PROJECT_ID>/quiz-images/quiz-frontend:<TAG>
gcloud builds submit ./backend \
--tag europe-west2-docker.pkg.dev/<PROJECT_ID>/quiz-images/quiz-backend:<TAG>



2. Apply Kubernetes manifests

kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/mongo.yaml
kubectl apply -f k8s/backend.yaml
kubectl apply -f k8s/frontend.yaml



3. Verify deployment

kubectl get pods -n quiz-prod
kubectl get svc -n quiz-prod



4. Access the application

Frontend (public):

http://<EXTERNAL-IP>:8080



Local Development (Docker Compose)

Run locally:

docker compose up --build

Access locally:

* Frontend: http://localhost:8080
* Backend: http://localhost:3000



📁 Project Structure

reactjs-quiz-app/
│
├── k8s/                # Kubernetes manifests
│   ├── namespace.yaml
│   ├── mongo.yaml
│   ├── backend.yaml
│   └── frontend.yaml
│
├── quiz-app/           # React frontend
├── backend/            # Node.js API
├── docker-compose.yml
└── README.md



🔐 Security Improvements

* Backend exposed internally using ClusterIP
* Frontend exposed via LoadBalancer
* No secrets stored in source control
* Service-to-service communication handled via Kubernetes DNS



🧠 Key Learnings

* Kubernetes service types (ClusterIP vs LoadBalancer)
* Internal vs external service communication
* Debugging container networking issues
* Deploying multi-tier apps on GKE
* Image management with Artifact Registry



📌 Future Improvements

* Add HTTPS using Ingress + SSL
* Introduce authentication (JWT)
* Use environment-based configuration
* Add CI/CD pipeline (GitHub Actions)
* Add monitoring/logging (Cloud Monitoring)



* badges (GCP, Docker, Kubernetes)
* or turn this into a portfolio project write-up

Just say 👍
# Argo CD GitOps test - Sun Apr 26 10:07:30 AM UTC 2026

## 🚀 CI/CD & GitOps Pipeline

This project implements a full GitOps workflow using Argo CD and Google Cloud Build.

### Flow

1. Developer pushes code to GitHub
2. Cloud Build is triggered automatically
3. Docker images are built and pushed to Artifact Registry
4. Kubernetes manifests reference the updated image
5. Argo CD detects changes in Git
6. Argo CD syncs the cluster state automatically

### Key Tools

- **Google Cloud Build** → CI pipeline
- **Artifact Registry** → container storage
- **Google Kubernetes Engine (GKE)** → runtime environment
- **Argo CD** → GitOps continuous deployment

### GitOps Principle

The cluster state is fully driven by Git.  
No manual `kubectl apply` is required after setup.

### Verification

- Cloud Build triggers on every push
- Argo CD shows `Healthy` and `Synced`
- Application updates automatically without manual intervention
