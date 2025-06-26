 
# Flask App with MySQL Docker Setup

This is a simple Flask app that interacts with a MySQL database. The app allows users to submit messages, which are then stored in the database and displayed on the frontend.

## Prerequisites

Before you begin, make sure you have the following installed:

- Docker
- Git (optional, for cloning the repository)

## Setup

1. Clone this repository (if you haven't already):

   ```bash
   git clone https://github.com/your-username/your-repo-name.git
   ```

2. Navigate to the project directory:

   ```bash
   cd your-repo-name
   ```

3. Create a `.env` file in the project directory to store your MySQL environment variables:

   ```bash
   touch .env
   ```

4. Open the `.env` file and add your MySQL configuration:

   ```
   MYSQL_HOST=mysql
   MYSQL_USER=your_username
   MYSQL_PASSWORD=your_password
   MYSQL_DB=your_database
   ```

## Usage

1. Start the containers using Docker Compose:

   ```bash
   docker-compose up --build
   ```

2. Access the Flask app in your web browser:

   - Frontend: http://localhost
   - Backend: http://localhost:5000

3. Create the `messages` table in your MySQL database:

   - Use a MySQL client or tool (e.g., phpMyAdmin) to execute the following SQL commands:
   
     ```sql
     CREATE TABLE messages (
         id INT AUTO_INCREMENT PRIMARY KEY,
         message TEXT
     );
     ```

4. Interact with the app:

   - Visit http://localhost to see the frontend. You can submit new messages using the form.
   - Visit http://localhost:5000/insert_sql to insert a message directly into the `messages` table via an SQL query.

## Cleaning Up

To stop and remove the Docker containers, press `Ctrl+C` in the terminal where the containers are running, or use the following command:

```bash
docker-compose down
```

## To run this two-tier application using  without docker-compose

- First create a docker image from Dockerfile
```bash
docker build -t flaskapp .
```

- Now, make sure that you have created a network using following command
```bash
docker network create twotier
```

- Attach both the containers in the same network, so that they can communicate with each other

i) MySQL container 
```bash
docker run -d \
    --name mysql \
    -v mysql-data:/var/lib/mysql \
    --network=twotier \
    -e MYSQL_DATABASE=mydb \
    -e MYSQL_ROOT_PASSWORD=admin \
    -p 3306:3306 \
    mysql:5.7

```
ii) Backend container
```bash
docker run -d \
    --name flaskapp \
    --network=twotier \
    -e MYSQL_HOST=mysql \
    -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=admin \
    -e MYSQL_DB=mydb \
    -p 5000:5000 \
    flaskapp:latest

```


# 🚀 Two-Tier Flask–MySQL Application on AWS EKS

This project showcases the deployment of a **cloud-native, containerized two-tier web application** using **Flask** (frontend) and **MySQL** (backend) on **Amazon EKS (Elastic Kubernetes Service)**. Built with DevOps best practices and infrastructure-as-code, this setup ensures scalability, modularity, and high availability on AWS Cloud.

---

## 🎯 Project Objective

To build and deploy a real-world, production-ready microservice application on Kubernetes using:
- **eksctl** for provisioning AWS EKS clusters
- **kubectl** and YAML manifests for managing application workloads
- **Docker** for containerization
- **Flask** and **MySQL** to simulate a real-world web service with persistent storage


## 🏗️ Architecture Overview
Client
│
▼
[Flask Web App - Frontend (Docker)]
│
▼
[MySQL Database - Backend (Docker)]
│
▼
[Kubernetes Cluster - Amazon EKS]
└── Managed Node Groups
└── IAM Roles, Networking (VPC, Subnets)

## 🧰 Tech Stack & Tools

| Category            | Tools/Tech                         |
|---------------------|------------------------------------|
| Cloud               | AWS EKS, IAM, VPC, EC2             |
| Containerization    | Docker                             |
| Container Orchestration | Kubernetes, kubectl, eksctl       |
| Application Layer   | Flask (Python)                     |
| Database Layer      | MySQL                              |
| IaC & Config        | Kubernetes YAML Manifests          |
| Monitoring (Future) | Metrics Server, Prometheus, Grafana|

✅ 1. Setup Prerequisites
Install eksctl, kubectl, AWS CLI
Configure AWS credentials: aws configure

✅ 2. Create EKS Cluster
      command: eksctl create cluster -f eks-manifests/cluster-config.yaml
✅ 3. Deploy App to EKS
      command:kubectl apply -f k8s/
✅ 4. Verify Resources
      command:kubectl get pods
              kubectl get svc
              kubectl get pvc
✅ 5. Access the Application
       command: kubectl port-forward svc/flask-service 5000:5000

🌱 Future Enhancements
  1.Integrate Jenkins or GitHub Actions for CI/CD
  2.Use Helm Charts for better deployment management
  3.Add Prometheus + Grafana for real-time monitoring
  4.Enable SSL termination + Ingress controller
  5.Persist MySQL data with AWS EBS volumes
  6.Deploy with ArgoCD for GitOps-based delivery

📄 License
This project is licensed under the MIT License. Feel free to fork and experiment




