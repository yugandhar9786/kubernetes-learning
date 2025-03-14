# Day 3/40 - Multi Stage Docker Build - Docker Tutorial For Beginners - CKA Full Course 2024 ☸️

# 🐳 TodoApp Docker Setup

This guide will walk you through setting up a Docker environment, building a Docker image for a sample Node.js-based TodoApp, and deploying it using Docker.

---

## 🚀 Pre-requisites
> If you have followed the Day 2 video and/or already have Docker Setup, you can skip this step.

If you would like to use Docker and Kubernetes sandbox environment, you can use the following platforms:
- [Play with Docker](https://labs.play-with-docker.com/)
- [Play with Kubernetes](https://labs.play-with-k8s.com/)

### Install Docker Desktop
- Download Docker Desktop client from: [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

---

## 🛠️ Getting started with the demo

### 1. Clone the sample repository  
Clone the sample TodoApp repository using the following command:

```bash
git clone https://github.com/piyushsachdeva/todoapp-docker.git

```bash
cd todoapp-docker/

## Create Dockerfile
```bash
touch Dockerfile

## Add Dockerfile Configuration
```bash

FROM node:18-alpine AS installer
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:latest AS deployer
COPY --from=installer /app/build /usr/share/nginx/html

## Build the Docker image
```bash
docker build -t todoapp-docker .


## Start the Docker container
```bash
docker run -dp 3000:3000 username/new-reponame:tagname

## Access the Docker container
```bash
docker exec -it containername sh
# OR
docker exec -it containerid sh





