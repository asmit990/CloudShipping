The traffic flows through a layered architecture to ensure security and efficient routing:

Internet → Nginx (Port 80) → Angular Frontend → Node.js Backend → MongoDB.

🛠 Tech Stack
Frontend: Angular 15

Backend: Node.js + Express

Database: MongoDB 7

Orchestration: Docker & Docker Compose

Web Server: Nginx (Reverse Proxy)

CI/CD: GitHub Actions

Cloud: AWS EC2 (Ubuntu 24.04, ap-south-1)

🐳 Dockerization Details
1. Backend (backend/Dockerfile)
Base Image: Node (Alpine)

Optimization: Dependencies installed using npm ci

Connection: Uses environment variable MONGO_URL=mongodb://mongo:27017/db

Internal Port: 8080

2. Frontend (frontend/Dockerfile)
Multi-stage Build:

Stage 1 (Build): Node image used for Angular production build

Stage 2 (Serve): Nginx image serves files from /usr/share/nginx/html

Exposed Port: 80

3. Database Setup
Uses official mongo:7 image

Configured in docker-compose.yml

Runs inside an isolated Docker network and is not publicly exposed.

🌐 Nginx Reverse Proxy
Nginx acts as the primary entry point, routing requests based on path:

/ → Angular Frontend

/api → Node Backend

Public URL: http://3.109.157.238

🔁 CI/CD Pipeline
The workflow in .github/workflows/deploy.yml automates the deployment on every push to main:

Build: Builds multi-architecture images (linux/amd64, linux/arm64)

Push: Uploads images to Docker Hub (asmit990/backend-app and asmit990/angular-15-crud)

Deploy: SSH into EC2, pulls latest images, and restarts containers via Docker Compose

📸 Project Assets & Proof
The following files and screenshots confirm the successful deployment and architecture:

Repository Structure
Plaintext
.
├── nginx.conf
├── docker-compose.yml
├── README.md
├── appworking.png
├── dockerhub-image.png
├── ec2-instance.png
├── github-actions-success.png
└── nginx-config.png
(As seen in project file structure)

Deployment Evidence
github-actions-success.png: Successful CI/CD Execution

dockerhub-image.png: Images built and pushed to Docker Hub

ec2-instance.png: AWS EC2 Instance running status

appworking.png: Application running via Public IP

nginx-config.png: Reverse proxy routing configuration

🔒 Security Considerations
DB Isolation: MongoDB is not publicly accessible.

Firewall: Only ports 22 (SSH) and 80 (HTTP) are open in Security Groups.

Secrets: SSH keys and Docker Hub credentials are stored in GitHub Secrets.

Environment Variables: Database connections use dynamic environment variables.
