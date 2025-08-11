# Jenkins Pipeline for Nginx Deployment

This repository contains a Jenkins pipeline that automates the deployment of a custom Nginx container with a simple `index.html` page to a remote server.

## Pipeline Overview

The pipeline performs the following steps:

1. **Clone Git Repository**: Clones this repository to the Jenkins workspace.
2. **Send index.html to Remote**: Copies the `index.html` file to the remote server.
3. **Build and Run Docker Container**: 
   - Creates a Dockerfile for a custom Nginx image
   - Builds the Docker image
   - Runs the container on the remote server, mapping port 8000 to the container's port 80

## Prerequisites

- Jenkins server with:
  - Git plugin installed
  - SSH access to the remote server
- Remote server with:
  - Docker installed and running
  - SSH access configured for the specified user
  - User must have permissions to run Docker commands

## Environment Variables

The pipeline uses the following environment variables:

| Variable      | Description                          | Example Value                     |
|---------------|--------------------------------------|-----------------------------------|
| `REMOTE_HOST` | IP address of the remote server      | `192.168.2.142`                  |
| `REMOTE_USER` | SSH user for the remote server       | `iman`                           |
| `REPO_URL`    | URL of the Git repository            | `https://github.com/ihrahimi/DevOps.git` |
| `IMAGE_NAME`  | Name for the Docker image            | `custom-nginx`                   |
| `CLONE_DIR`   | Local directory for repository clone | `devops-repo`                    |

## File Structure

- `jenkins/nginx-simple/index.html`: The HTML file that will be served by Nginx
- `Jenkinsfile`: The pipeline definition file

## How It Works

1. The pipeline clones the repository to get access to the `index.html` file
2. The file is copied to the remote server via SCP
3. On the remote server:
   - A Dockerfile is created using the official Nginx image
   - The `index.html` is added to the image
   - Any existing container with the same name is stopped and removed
   - A new container is built and run

## Accessing the Application

After successful pipeline execution, the Nginx server will be accessible at:
http://<REMOTE_HOST>:8000


## Failure Handling

The pipeline includes error checking for:
- Missing `index.html` file
- Docker build failures
- File transfer failures

Post-build actions provide success/failure notifications.

## Customization

To use this pipeline with your own setup:
1. Update the environment variables in the Jenkinsfile
2. Replace the `index.html` with your own content
3. Adjust port mappings if needed
