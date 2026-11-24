# ZeetaFin ERP Frontend – Docker Containerization and Amazon ECR Deployment

## Project Overview

This project demonstrates the containerization of the ZeetaFin ERP frontend application using Docker and the deployment of the resulting Docker image to Amazon Elastic Container Registry (Amazon ECR).

The application is built with Next.js, Docker is used to package the application and its runtime dependencies into a portable container image.

## Technologies Used

- Next.js 14
- React 18
- Node.js 20
- Docker
- Amazon Elastic Container Registry (ECR)
- AWS CLI

## Application Details

| Item | Description |
|---|---|
| Application | ZeetaFin ERP Frontend |
| Framework | Next.js |
| Package Manager | npm |
| Application Port | 3000 |
| Container Runtime | Node.js |
| Start Command | node server.js |
| Docker Image | zeetafin-fe:latest |

## 1. Docker Configuration

A multi stage Dockerfile was used to build and package the application.

The Dockerfile:
- Uses Node.js 20 Alpine as the base image.
- Installs and prepares application dependencies.
- Builds the Next.js application.
- Uses Next.js standalone output to reduce the runtime image.
- Creates a non root user for running the application.
- Exposes port 3000.
- Starts the application using node server.js.

The next.config.js file is configured with:

```javascript
const nextConfig = {
  output: 'standalone',
  images: { domains: [] },
};

module.exports = nextConfig;
```

## 2. Docker Ignore Configuration

A .dockerignore file was created to prevent unnecessary files and sensitive environment files from being included in the Docker build context.

Excluded files include:

- node_modules
- .next
- .env
- .env*.local
- .git
- Log files

Environment files containing credentials or secrets should never be committed to GitHub or included in the container image.

## 3. Build the Docker Image

From the frontend project directory, run:


docker build -t zeetafin-fe:latest .


## 4. Run the Application Locally

Run the Docker container:

docker run -p 80:3000 zeetafin-fe:latest

Open the application in a browser:

http://localhost


The application listens on port 3000 inside the container, while port 80 is exposed on the host machine.

To check running containers:

docker ps

To stop the container, press Ctrl + C in the terminal running it.

## 5. Amazon ECR Configuration

The Docker image was deployed to an existing Amazon ECR repository.

| Configuration | Value |
|---|---|
| AWS Region | eu-west-1 |
| AWS Account ID | 865480028999 |
| ECR Repository | xahra/zeetafin |
| Image Tag | latest |

**ECR Image URI:**

865480028999.dkr.ecr.eu-west-1.amazonaws.com/xahra/zeetafin:latest


## 6. Authenticate Docker with Amazon ECR

Authenticate Docker using the AWS CLI:

aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin 865480028999.dkr.ecr.eu-west-1.amazonaws.com

A successful authentication returns:

Login Succeeded


## 7. Tag the Docker Image

Tag the locally built image for the ECR repository:

docker tag zeetafin-fe:latest 865480028999.dkr.ecr.eu-west-1.amazonaws.com/xahra/zeetafin:latest

## 8. Push the Image to Amazon ECR

Push the tagged image:

docker push 865480028999.dkr.ecr.eu-west-1.amazonaws.com/xahra/zeetafin:latest


## 9. Verify the Image in Amazon ECR

The image was verified using:
aws ecr list-images --repository-name xahra/zeetafin --region eu-west-1


The repository returned the latest image tag and its image digest.

The image can also be viewed in the AWS Management Console under:

**Amazon ECR → Private registry → Repositories → xahra/zeetafin**



## Conclusion

This project provided practical experience in application containerization using Docker and deploying container images to Amazon ECR.

It demonstrated how to package a Next.js application into a Docker image, run it locally, authenticate with AWS, and push the image to a private Amazon ECR repository.