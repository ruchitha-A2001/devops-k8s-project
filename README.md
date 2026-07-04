\# Flask App Deployment on AWS ECS Fargate with Terraform \& CI/CD



A DevOps project demonstrating real cloud deployment using AWS ECS Fargate, ECR, CloudWatch, Terraform, and GitHub Actions CI/CD pipeline.



\## Architecture



```

GitHub Push

&#x20;    ↓

GitHub Actions (CI/CD)

&#x20;    ↓

Build Docker Image

&#x20;    ↓

Push to AWS ECR (Container Registry)

&#x20;    ↓

Deploy to AWS ECS Fargate (Serverless Containers)

&#x20;    ↓

CloudWatch (Monitoring + Logs)

```



\## Tech Stack



| Tool | Purpose |

|---|---|

| Python Flask | Application |

| Docker | Containerization |

| AWS ECR | Private container registry |

| AWS ECS Fargate | Serverless container orchestration |

| AWS CloudWatch | Monitoring and logging |

| Terraform | Infrastructure as Code |

| GitHub Actions | CI/CD pipeline automation |



\## Project Structure



```

.

├── app.py                          # Flask application

├── requirements.txt                # Python dependencies

├── Dockerfile                      # Container image definition

├── main.tf                         # Terraform AWS provider configuration

├── ecr.tf                          # ECR repository definition

├── ecs.tf                          # ECS cluster, task definition, service, IAM roles

└── .github/workflows/ci-cd.yaml   # GitHub Actions CI/CD pipeline

```



\## AWS Infrastructure (Terraform-managed)



\- \*\*ECR Repository\*\* — private Docker image registry with image scanning enabled

\- \*\*ECS Cluster\*\* — Fargate-based serverless container cluster

\- \*\*ECS Task Definition\*\* — defines container specs (CPU: 256, Memory: 512MB)

\- \*\*ECS Service\*\* — maintains desired count of running tasks

\- \*\*IAM Role\*\* — ECS task execution role with least-privilege permissions

\- \*\*Security Group\*\* — controls inbound/outbound traffic to containers

\- \*\*CloudWatch Log Group\*\* — centralised logging with 7-day retention



\## CI/CD Pipeline



On every push to `main` branch, GitHub Actions automatically:

1\. Configures AWS credentials securely via GitHub Secrets

2\. Authenticates to Amazon ECR

3\. Builds a new Docker image tagged with the Git commit SHA

4\. Pushes the image to ECR

5\. Triggers a new ECS deployment with the updated image



\## How to Run



\### Prerequisites

\- AWS CLI configured (`aws configure`)

\- Docker Desktop

\- Terraform



\### Steps



```bash

\# 1. Clone the repository

git clone https://github.com/ruchithaandra/devops-ecs-project.git

cd devops-ecs-project



\# 2. Initialize Terraform

terraform init



\# 3. Review the plan

terraform plan



\# 4. Deploy infrastructure

terraform apply



\# 5. Authenticate Docker to ECR

$token = aws ecr get-login-password --region ap-south-1

docker login --username AWS --password "$token" <your-account-id>.dkr.ecr.ap-south-1.amazonaws.com



\# 6. Build and push Docker image

docker build -t flask-devops-app:v1 .

docker tag flask-devops-app:v1 <ecr-repo-url>:v1

docker push <ecr-repo-url>:v1

```



\## Monitoring



Application logs are streamed to AWS CloudWatch under:

```

Log Group: /ecs/flask-devops-app

```



View logs via AWS Console → CloudWatch → Log Groups → `/ecs/flask-devops-app`



\## Cleanup



To avoid AWS charges, destroy all resources when done:

```bash

terraform destroy

```



\## Key DevOps Concepts Demonstrated



\- \*\*Infrastructure as Code\*\* — all AWS resources defined and managed via Terraform

\- \*\*Containerization\*\* — app packaged as a Docker image for consistent deployments

\- \*\*Serverless containers\*\* — ECS Fargate eliminates server management overhead

\- \*\*CI/CD automation\*\* — every code push triggers an automated build and deployment

\- \*\*Observability\*\* — CloudWatch logs provide visibility into running containers

\- \*\*Security\*\* — IAM roles with least privilege, private ECR registry, security groups



\## Future Improvements



\- Add an Application Load Balancer (ALB) for production-grade traffic management

\- Implement auto-scaling based on CloudWatch metrics

\- Add Terraform remote state using S3 + DynamoDB

\- Add health check endpoint monitoring with CloudWatch alarms

\- Integrate AWS CodeDeploy for Blue/Green deployments

