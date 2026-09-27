# Deploy locally from Amazon ECR

This deployment runs the application on a local Docker engine while pulling the
prebuilt backend and frontend images from private Amazon ECR repositories. It
does not build application images locally.

## Prerequisites

- Docker with Docker Compose v2
- AWS CLI
- AWS credentials with permission to authenticate to ECR and pull from the
  `backend` and `frontend` repositories in account `788210522308`
- Images already published by the GitHub Actions pipeline

## 1. Configure AWS credentials

Use an existing AWS CLI profile:

```bash
aws configure --profile k8s-learning
export AWS_PROFILE=k8s-learning
```

If your default AWS CLI credentials already have ECR access, you can omit
`--profile` and `AWS_PROFILE`.

## 2. Log Docker in to ECR

ECR login tokens expire after 12 hours, so repeat this command when a pull
reports an authentication error.

```bash
export AWS_REGION=us-east-1
export ECR_REGISTRY=788210522308.dkr.ecr.us-east-1.amazonaws.com

aws ecr get-login-password --region "$AWS_REGION" \
  | docker login --username AWS --password-stdin "$ECR_REGISTRY"
```

## 3. Pull and start the application

Run these commands from the `k8s-learning-app` directory:

```bash
docker compose -f docker-compose.ecr.yml pull
docker compose -f docker-compose.ecr.yml up -d
```

Compose pulls the `latest` backend and frontend images by default. To deploy
the immutable image tags created for a particular Git commit instead:

```bash
export IMAGE_TAG=<full-git-commit-sha>
docker compose -f docker-compose.ecr.yml pull
docker compose -f docker-compose.ecr.yml up -d
```

## 4. Verify the deployment

```bash
docker compose -f docker-compose.ecr.yml ps
docker compose -f docker-compose.ecr.yml logs -f
```

Open <http://localhost:8080> and sign in with:

```text
Email: admin@example.com
Password: admin-password
```

Press `Ctrl+C` to stop following the logs; the containers continue running in
the background.

## Deploy a newer image

Log in to ECR again if necessary, then pull and recreate the services:

```bash
docker compose -f docker-compose.ecr.yml pull
docker compose -f docker-compose.ecr.yml up -d --remove-orphans
```

## Stop or reset the deployment

Stop the containers while retaining PostgreSQL data:

```bash
docker compose -f docker-compose.ecr.yml down
```

Delete the containers and PostgreSQL volume for a clean reset:

```bash
docker compose -f docker-compose.ecr.yml down --volumes
```

The second command permanently deletes the local application database.

## Override local settings

Compose accepts environment variables for local configuration. For example:

```bash
export JWT_SECRET="replace-with-a-random-value-at-least-32-characters"
export DEFAULT_ADMIN_EMAIL="owner@example.com"
export DEFAULT_ADMIN_PASSWORD="replace-with-a-strong-password"
docker compose -f docker-compose.ecr.yml up -d
```

Do not commit real credentials or production secrets to this repository.
