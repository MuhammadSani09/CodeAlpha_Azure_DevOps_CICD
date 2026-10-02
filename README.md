# Azure DevOps CI/CD Pipeline — CodeAlpha Task 1

## Overview
This project automates the building, testing, and deployment of a
containerized website using Azure Pipelines, Azure Container Registry,
and Azure App Service.

## Technologies
- Git and Azure Repos
- Azure Pipelines
- Docker and Nginx
- Azure Container Registry (ACR)
- Azure App Service
- Microsoft Entra workload identity federation
- Managed identity

## Pipeline Workflow
When changes are committed to the main branch in Azure Repos:

1. Azure Pipelines checks out the source code.
2. Docker builds the website image.
3. A temporary container runs the website.
4. An HTTP check verifies that the page responds and contains the
   expected heading.
5. The tested image is tagged with the pipeline build ID and pushed
   to Azure Container Registry.
6. The pipeline updates the App Service main container to use that image.

## Project Files
- `index.html` — website content.
- `Dockerfile` — packages the website using Nginx.
- `azure-pipelines.yml` — build, test, push, and deployment configuration.
- `README.md` — project documentation.

## Authentication and Security
- The pipeline authenticates to Azure through a service connection
  using workload identity federation.
- The App Service uses its managed identity to pull images from ACR.
- Credentials are not stored in this repository.
- Azure permissions and service connections must be configured separately.

## Run Locally
With Docker installed and running, execute these commands from the
project directory:

    docker build -t internship-web:local .
    docker run --rm -d --name internship-web-local -p 8080:80 internship-web:local

Open http://localhost:8080 in a browser.

Stop the container when finished:

    docker stop internship-web-local

## Deployment Setup
To reproduce the Azure deployment:

1. Create an Azure Container Registry and a Linux Azure App Service
   configured for a main container.
2. Configure an Azure Resource Manager service connection using
   workload identity federation.
3. Grant the pipeline identity permission to push images to the registry
   and update the web app.
4. Enable the web app's managed identity and grant it AcrPull access
   to the registry.
5. Update the service connection, registry, web app, and resource group
   names in the YAML to match your environment.
6. Create an Azure Pipeline using the YAML file and authorize its
   service connection.

The main container is named `main` and serves traffic on port 80.

## Testing and Troubleshooting
The pipeline checks the website using curl and verifies the heading
"DevOps Pipeline is Working!".

During initial testing, the HTTP request reached the container before
Nginx was ready. Retry options were added to handle startup timing.

The test step prints container logs and removes the temporary container
when it finishes.

## Results
- The container build and website check completed successfully.
- The image was stored in Azure Container Registry.
- The website was deployed to Azure App Service.
- A Version 2 webpage update was confirmed after a successful
  deployment pipeline run.

## Repository Note
This GitHub repository is a submission copy of the project.
The working pipeline uses the original Azure Repos repository.
Changes made only to this GitHub copy do not trigger that pipeline.

## Author
Muhammad Sani
CodeAlpha — DevOps Internship
