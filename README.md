CloudSec Pipeline

Overview

This project demonstrates a completely end-to-end cloud-native DevOps pipeline, using Infrastructure as Code to deploy a containerized React application to AWS, with CI/CD automation.
The system provisioning of AWS infrastructure with Terraform, application development with Docker, and image storage in Amazon ECR, and deployment of both to ECS Fargate behind an Application Load Balancer. GitHub Actions are used to deploy fully automated.

![alt text](<AWS cloud architecture with GitHub Actions.png>)

What this Project Shows.

Design and implementation of end-to-end DevOps pipeline.
Provisioning of infrastructure with Terraform (IaC).
Ready-to-use production Docker and containerization.
AWS ECS Fargate with load balancing.
CI/CD automation with GitHub Actions.
Troubleshooting and troubleshooting actual deployment.
Reproducible, version-controlled deployments, and rollback.

Repository Structure

ecs-assignment/
│
├── .github/
│   └── workflows/
│       └── deploy.yml        
│
├── terraform/
│   └── envs/
│       └── dev/
│           ├── main.tf       
│           ├── terraform.tfvars
│           └── provider.tf   
│
├── src/                      
│
├── public/                   
│
├── Dockerfile               
├── nginx.conf              
│
├── package.json
├── package-lock.json
├── tsconfig.json
│
├── .dockerignore
├── .gitignore
│
└── README.md

CI/CD Pipeline Flow
Developer → GitHub → GitHub Actions
                    ↓
              Build Docker Image
                    ↓
              Tag (commit SHA)
                    ↓
              Push to Amazon ECR
                    ↓
        Update ECS Task Definition
                    ↓
              Deploy to ECS Service

Tech Stack

Frontend

    React (TypeScript)
    DevOps & Cloud
    AWS ECS (Fargate)
    Amazon ECR
    Application Load Balancer (ALB)
    AWS VPC, Subnets, Security Groups.
    AWS IAM
    CloudWatch Logs

Infrastructure as Code

    Terraform

CI/CD

    GitHub Actions

Containerization
    - Docker
    - Nginx


⚙️ Infrastructure (Terraform)
Terraform is used to define infrastructure completely, making it consistent and reproducible.

Key Resources:

    - VPC (custom CIDR block)
    - Public subnets in more than one AZ.
    - Internet Gateway
    - Route tables
    - Security groups
    - Application Load Balancer (facing internet)
    - Target group & listener
    - ECS cluster
    - ECS task definition & service
    - IAM roles (execution role)
    - CloudWatch log group

Location:
terraform/envs/dev/main.tf

Containerization Strategy

Docker is used to productionise the React application:

    - Multi-stage build (build- serve)
    - Static files generated using React build
    - Served via Nginx
    - SPA routing (resolves refresh problems) configured.
    - ECS compatible on port 80.

CI/CD Pipeline (GitHub Actions)

Completely automated deployment pipeline on each push to main.

Pipeline Breakdown:

    1. Code Push
    2. Build Docker Image
    3. Tag Image with Commit SHA.
    4. Push Image to Amazon ECR.
    5. Retrieve Current ECS Task Definition.
    6. Infuse New Image Version.
    7. Register New Task Definition Revision
    8. Implement Updated Service to ECS.

Key DevOps Features

    * Completely automated CI/CD pipeline.
    Image tag commit deployments with image tag commit deployments.
    * Versions def task definition.
    * Safe and instant rollback.
    * Terraform (Infrastructure as Code)
    * Load-balanced production deployment
    Centralised cloudwatch logging.


Deployment & Rollback Strategy

Every deployment results in a new revision of an ECS task definition:

Example:
    - app-task:7
    - app-task:8
    - app-task:9

Rollback Process:

    2. Go to ECS Service.
    2. Click Update
    3. Choose past task definition.
    4. Deploy

This allows quick and secure recovery of failed deployments.

Challenges & Solutions

React SPA Routing Problem.

Issue: Refresh resulted in 404 errors.
Mean: Set up Nginx to direct all requests to index.html.

Version Confusion with Docker Image.

problem: With latest, inconsistent deployments were made.
Solution: Installed image tagging with commit.

ECS Deployment Failure to update.

Problem: No new pictures were used.
Solution: New CI/CD pipe to dynamically specify tasks.

CI/CD Failures

Problem: Early pipeline failures because of config/auth problems.
Solution: Checked workflow and resolved logs.

▶️ Local Development
# Build image
docker build -t threat-composer-app.

# Run container
docker run -p 3000:80 threat-composer-app.
Access:
http://localhost:3000

Future Improvements:

HTTPS with AWS ACM
Infrastructure ℊ CloudWatch alarms and monitoring.
Multi-environment (dev / prod) setup.
Remote Terraform state (S3 + DynamoDB)
CI/CD testing and security checks.
Migrate ECS activities to private subnets.

Key Takeaways

This project demonstrates:

    - Planning and implementing scalable cloud infrastructure.
    - Adopting CI/CD pipelines by putting them into practice.
    - Troubleshooting production-like issues
    - Adopting the DevOps principle on an end-to-end basis.
    - Design of reproducible and automated system.