# 2048
# Dockerized 2048 Game with AWS Deployment

This project demonstrates how to containerize the 2048 game using Docker and deploy it to the cloud using AWS Elastic Beanstalk. By following the steps outlined below, you can replicate the entire process locally and on the cloud.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Technologies Used](#technologies-used)
- [Project Setup](#project-setup)
- [Dockerizing the 2048 Game](#dockerizing-the-2048-game)
- [Running Locally](#running-locally)
- [Deploying to AWS Elastic Beanstalk](#deploying-to-aws-elastic-beanstalk)
- [License](#license)

## Prerequisites
Before you begin, ensure you have the following tools installed on your machine:
- Docker
- AWS Account
- AWS CLI configured with credentials

## Technologies Used
- **Docker**: To create and run containers for the 2048 game.
- **AWS Elastic Beanstalk**: For deploying and managing the application in the cloud.

## Project Setup
1. Clone this repository:
    ```bash
    git clone https://github.com/gabrielecirulli/2048
    ```
2. Navigate to the project directory:
    ```bash
    cd docker-2048-game
    ```

## Dockerizing the 2048 Game
1. Create a Dockerfile for the project using Ubuntu as the base image:
    ```dockerfile
    FROM ubuntu:22.04

    # Update the package list and install required utilities
    RUN apt-get update && \
        apt-get install -y nginx zip curl

    # Configure NGINX
    RUN echo "daemon off;" >> /etc/nginx/nginx.conf

    # Download the 2048 game repository from GitHub
    RUN curl -o /var/www/html/master.zip -L https://github.com/gabrielecirulli/2048/archive/refs/heads/master.zip

    # Unzip the game files
    RUN unzip /var/www/html/master.zip -d /var/www/html/ && \
        mv /var/www/html/2048-master/* /var/www/html/ && \
        rm -rf /var/www/html/2048-master /var/www/html/master.zip

    # Expose port 80
    EXPOSE 80

    # Command to start NGINX
    CMD ["nginx", "-g", "daemon off;"]
    ```

2. Build the Docker image:
    ```bash
    docker build -t 2048-game .
    ```

## Running Locally
Once the Docker image is built, you can run the container locally to test the game:

```bash
docker run -d -p 80:80 2048-game
