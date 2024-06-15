# Sample# Sample Fullstack Application



## Overview

This repository contains a full stack application with containerized components. The application uses Docker for containerization and GitHub Actions for CI/CD to build and push images to GitHub Container Registry (GHCR).

## Prerequisites

- Docker
- Docker Compose
- GitHub account

## Setup

### Fork the Repository

1. Fork this repository to your GitHub account.

### Clone the Repository

2. Clone the forked repository to your local machine:

    ```bash
    git clone https:/Sangram0105/github.com//sample-fullstack-application.git
    cd sample-fullstack-application
    ```

### Environment Variables

3. Create a `.env` file at the root of the project and add the following environment variables:

    ```dotenv
    NEXT_PUBLIC_BACKEND_BASE_URL=http://localhost:5000
    DATABASE_URL=postgres://postgres:<Type same password entered in POSTGRES_PASSWORD>@db:5432/db_01
    REDIS_URL=redis://redis:6379
    POSTGRES_PASSWORD=<your_postgres_password>
    POSTGRES_DB=db_01
    BACKEND_BASE_URL=http://localhost:5000
    ```

## Docker Configuration

### Dockerfiles

The project is structured with separate Dockerfiles for each component:

- **Backend**: `/backend/Dockerfile`
- **Frontend**: `/frontend/Dockerfile`
- **Database**: `/database/Dockerfile`
- **Redis**: `/redis/Dockerfile`

### Docker Compose

The `docker-compose.yml` file at the root level of the repository orchestrates all the services:

```yaml
version: '3.8'

services:
  frontend:
    image: ghcr.io/Sangram0105/frontend:latest
    ports:
      - "3000:3000"
    environment:
      - NEXT_PUBLIC_BACKEND_BASE_URL=${NEXT_PUBLIC_BACKEND_BASE_URL}
      - DATABASE_URL=${DATABASE_URL}
      - REDIS_URL=${REDIS_URL}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_DB=${POSTGRES_DB}
      - BACKEND_BASE_URL=${BACKEND_BASE_URL}
    depends_on:
      - backend

  backend:
    image: ghcr.io/Sangram0105/backend:latest
    ports:
      - "5000:5000"
    environment:
      - DATABASE_URL=${DATABASE_URL}
      - REDIS_URL=${REDIS_URL}
    depends_on:
      - database
      - redis

  database:
    image: postgres:13
    environment:
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_DB=${POSTGRES_DB}
    ports:
      - "5432:5432"

  redis:
    image: redis:6
    ports:
      - "6379:6379"
```
# Running the Project

This document outlines how to run the project locally using Docker Compose.

## Prerequisites

* Ensure you have Docker and Docker Compose installed. You can find installation instructions on their official websites:
    * Docker: https://docs.docker.com/engine/install/
    * Docker Compose: https://docs.docker.com/compose/install/

## Running the Application

1. **Navigate:** Open your terminal and navigate to the root directory of your project.
2. **Start Services:** Run the following command to start all services defined in your `docker-compose.yml` file:

```bash
docker-compose up

```
## Accessing the Application

**Frontend**: http://localhost:3000.

**Backend**: http://localhost:5000.

**Database**: localhost:5432 (Username: postgres, Password: <your_postgres_password>).

**Redis**: localhost:6379.

## Common Issues
**1. Port Conflicts**

If the default ports (3000, 5000, 5432, 6379) are already in use by other applications, you can change them in the docker-compose.yml file.

**2. Environment Variables**

Ensure that all required environment variables are correctly defined in the .env file.



This `README.md` file provides clear instructions for setting up, running, and contributing to the project. Ensure you replace `<your_postgres_password>` with your actual PostgreSQL password. Additionally, make sure your GitHub Actions workflows are set up correctly and that you have the necessary permissions and tokens in your GitHub repository settings for GHCR.



