# FastAPI Kubernetes Deployment

This project demonstrates how to deploy a FastAPI application on Kubernetes with MySQL as the database.

## Features
- FastAPI application with RESTful endpoints.
- MySQL database deployed with a StatefulSet.
- Kubernetes Deployment and Service for FastAPI.
- Dockerized FastAPI application.

## Prerequisites
- Kubernetes cluster.
- kubectl configured to manage your cluster.
- Docker installed locally.

## Deployment Steps
1. Build and push the Docker image:
   ```bash
   docker build -t <your-dockerhub-username>/fastapi:latest .
   docker push <your-dockerhub-username>/fastapi:latest
