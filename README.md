# 🚀 Containerized Microservices on Amazon ECS Fargate

## 📌 Overview

This project demonstrates how to migrate a monolithic **Node.js** application into a modern **microservices architecture** using **Amazon ECS Fargate**.

The application consists of three independent services:

* 🔐 Auth Service
* 📦 Orders Service
* 📧 Notifications Service

Each service runs in its own Docker container on Amazon ECS Fargate and communicates with other services using **AWS Cloud Map** for DNS-based service discovery.

The solution follows AWS Well-Architected best practices by implementing secure secret management, blue/green deployments, centralized logging, distributed tracing, and infrastructure automation.

---

# 🏗 Architecture

[https://raw.githubusercontent.com/amrsamir6080/ecs-fargate-microservices/main/architecture/ecs-fargate-microservices.png](https://github.com/amrsamir6080/ecs-fargate-microservices/blob/main/Architecture/architectureecs-fargate-microservices.png)

---

# 🖥 Architecture Components

```
Users
   │
   ▼
Application Load Balancer
   │
   ├───────────────┐
   │               │
/api/auth      /api/orders
   │               │
   ▼               ▼
Auth Service    Orders Service
        │
        ▼
Notifications Service

        │
AWS Cloud Map

        │
Amazon ElastiCache Redis

        │
AWS Secrets Manager

        │
CloudWatch + X-Ray
```

---

# ☁ AWS Services Used

| Service                      | Purpose                                             |
| ---------------------------- | --------------------------------------------------- |
| Amazon ECS Fargate           | Run containers without managing EC2 instances       |
| Amazon ECR                   | Store Docker container images                       |
| Application Load Balancer    | Path-based routing to ECS services                  |
| AWS Cloud Map                | DNS-based service discovery                         |
| AWS Secrets Manager          | Secure storage of database credentials and API keys |
| Amazon ElastiCache for Redis | Shared cache and session storage                    |
| AWS CodePipeline             | Continuous Integration & Continuous Delivery        |
| AWS CodeBuild                | Build Docker images                                 |
| AWS CodeDeploy               | Blue/Green deployments                              |
| Amazon CloudWatch            | Centralized logging and monitoring                  |
| AWS X-Ray                    | Distributed tracing                                 |
| IAM                          | Secure permissions for AWS resources                |
| Amazon VPC                   | Networking and isolation                            |

---

# 📂 Project Structure

```
ecs-fargate-microservices/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── architecture/
│   ├── ecs-fargate-microservices.png
│   ├── architecture.drawio
│   └── architecture.pdf
│
├── app/
│   ├── auth/
│   ├── orders/
│   └── notifications/
│
├── docker/
│
├── infrastructure/
│   └── terraform/
│
├── ecs/
│
├── pipeline/
│
├── docs/
│
└── screenshots/
```

---

# 🔄 CI/CD Pipeline

```
Developer

     │

GitHub Repository

     │

AWS CodePipeline

     │

AWS CodeBuild

(Build Docker Images)

     │

Amazon ECR

(Container Images)

     │

AWS CodeDeploy

(Blue/Green Deployment)

     │

Amazon ECS Fargate
```

---

# 🌐 Application Routing

| URL                | Destination           |
| ------------------ | --------------------- |
| /api/auth          | Auth Service          |
| /api/orders        | Orders Service        |
| /api/notifications | Notifications Service |

---

# 🔍 Service Discovery

Instead of hardcoding IP addresses, each service is automatically registered with **AWS Cloud Map**.

Example:

```
orders-service
      │
      ▼
auth-service.internal

notifications-service.internal
```

This allows containers to communicate using DNS names.

---

# 🔐 Secret Management

Application secrets are stored securely in **AWS Secrets Manager**.

Examples include:

* Database username
* Database password
* JWT Secret
* SMTP credentials
* Third-party API keys

Secrets are injected into ECS tasks at runtime.

---

# 🐳 Container Images

Each microservice has its own Docker image.

```
Auth Service
↓

Amazon ECR

Orders Service
↓

Amazon ECR

Notifications Service
↓

Amazon ECR
```

Amazon ECR performs image vulnerability scanning on every push.

---

# 📈 Monitoring & Observability

The project uses:

* Amazon CloudWatch Logs
* Amazon CloudWatch Metrics
* AWS X-Ray Distributed Tracing

X-Ray enables end-to-end tracing across all microservices.

---

# 🚀 Blue/Green Deployment

Deployments are handled by **AWS CodeDeploy**.

Features:

* Zero downtime deployment
* Automatic traffic shifting
* Health checks
* Automatic rollback on failure

Deployment Flow:

```
Current Version (Blue)

↓

Deploy New Version (Green)

↓

Health Checks

↓

Traffic Shift

↓

Terminate Old Version
```

---

# 🗄 Session Management

Amazon ElastiCache for Redis is used as a centralized session store.

Benefits:

* Stateless containers
* Faster response times
* Shared session storage
* Improved scalability

---

# 🔑 IAM Roles

The solution uses separate IAM roles:

### ECS Task Execution Role

Permissions:

* Pull images from Amazon ECR
* Write logs to CloudWatch

### ECS Task Role

Permissions:

* Read Secrets Manager
* Access Cloud Map
* Send traces to X-Ray
* Connect to Redis

---

# 📊 Features

* Dockerized Node.js Microservices
* Amazon ECS Fargate
* Application Load Balancer
* Path-based Routing
* AWS Cloud Map
* Amazon ECR
* Secrets Manager
* ElastiCache Redis
* Blue/Green Deployments
* CloudWatch Monitoring
* AWS X-Ray Tracing
* Infrastructure as Code (Terraform)
* Zero Downtime Deployment
* Automatic Rollback
* Secure IAM Roles

---

# 🛠 Deployment

## Clone Repository

```bash
git clone https://github.com/<your-username>/ecs-fargate-microservices.git

cd ecs-fargate-microservices
```

## Build Docker Images

```bash
docker build -t auth-service ./app/auth

docker build -t orders-service ./app/orders

docker build -t notifications-service ./app/notifications
```

## Push Images

```bash
docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/auth-service

docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/orders-service

docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/notifications-service
```

## Deploy Infrastructure

```bash
cd infrastructure/terraform

terraform init

terraform plan

terraform apply
```

---

# 📷 Screenshots

Include screenshots after deployment:

* ECS Cluster
* ECS Services
* Running Tasks
* Amazon ECR
* Application Load Balancer
* Target Groups
* AWS Cloud Map
* Secrets Manager
* ElastiCache Redis
* CloudWatch Logs
* AWS X-Ray Service Map
* CodePipeline
* CodeDeploy

---

# 🎯 Learning Outcomes

This project demonstrates practical experience with:

* Amazon ECS Fargate
* Docker Containers
* Microservices Architecture
* Amazon ECR
* AWS Cloud Map
* Secrets Management
* Infrastructure as Code
* Blue/Green Deployments
* Distributed Tracing
* Continuous Delivery
* High Availability
* Stateless Applications

---

# 📚 Future Improvements

* Amazon Route 53
* AWS WAF
* AWS Shield
* Auto Scaling Policies
* HTTPS using ACM
* Multi-AZ Redis
* Canary Deployments
* Amazon EventBridge
* AWS Lambda Integrations

---

# 👨‍💻 Author

**Amr Samir**

DevOps Engineer

AWS • Docker • Kubernetes • Terraform • Linux • CI/CD

---

# ⭐ Support

If you found this project useful, consider giving it a ⭐ on GitHub.
