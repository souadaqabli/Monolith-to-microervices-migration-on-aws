#  Building Microservices and a CI/CD Pipeline with AWS

## 📌 Context & Objective
The goal of this project was to refactor a legacy monolithic Node.js application into a modern, scalable microservices architecture. 

The main challenge was to decouple the existing monolithic codebase into independent services and implement a fully automated CI/CD pipeline enabling Blue/Green deployments for zero-downtime releases.

## 🏗️ Architecture Diagram
![AWS CI/CD and Microservices Architecture](C:\Users\hp\Desktop)

## 🛠️ Tech Stack & AWS Services
* **Compute Containerization:** Amazon ECS on Fargate, Docker, Amazon ECR
* **Network & Traffic Routing:** Amazon VPC, Application Load Balancer (ALB), Target Groups
* **Database:** Amazon RDS (MySQL)
* **CI/CD Pipeline:** AWS CodePipeline, AWS CodeDeploy, AWS CodeCommit
* **Observability:** Amazon CloudWatch
* **Development:** AWS Cloud9 IDE

## 🚀 Key Implementation Details

### 1. Monolith to Microservices Decomposition
The legacy application was split into two distinct, independently deployable services:
* **Customer Service:** Provides read-only access to the coffee suppliers catalog.
* **Employee Service:** Provides administrative capabilities to add, update, or remove supplier records. 

### 2. Intelligent Traffic Routing
An Application Load Balancer (ALB) was configured to act as the single entry point, routing traffic based on URL path rules:
* Requests containing `/admin/*` are forwarded to the **Employee Service** target groups.
* All other requests are routed to the **Customer Service** target groups.

### 3. Serverless Compute with ECS Fargate
Both microservices are containerized using Docker and hosted on Amazon ECS using the serverless Fargate launch type. This eliminates the need to manage underlying EC2 instances while allowing independent scaling for each service.

### 4. Zero-Downtime CI/CD Pipeline
A robust continuous integration and deployment pipeline was built using AWS native developer tools:
* **Source:** Code changes are pushed to AWS CodeCommit, triggering the pipeline.
* **Artifacts:** New Docker images are built and pushed to Amazon ECR.
* **Deployment (Blue/Green):** AWS CodeDeploy orchestrates the shift of traffic from the original container set (Blue) to the new replacement container set (Green). This ensures that the new version is fully operational before receiving production traffic.

## 💡 Challenges Overcome
* **Port Conflict Management:** During local testing, resolved port conflicts by assigning specific ports (8080 and 8081) to different microservices before standardizing them for ECS deployment.
* **Security & Isolation:** Implemented strict security group rules and customized ALB listener rules to restrict access to the Employee (Admin) service to authorized IP addresses only.
* **Dynamic Configuration:** Engineered the `taskdef.json` and `appspec.yaml` files to accept dynamic image names injected by CodePipeline at runtime.
