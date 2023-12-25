## Project Overview
This repository contains the source code and configuration files for Java-Maven-App. It leverages Jenkins for continuous integration and delivery. The Jenkinsfile in the master branch of the repository defines the pipeline for building, testing, and deploying the application.

# Prerequisites

Make sure you have the following prerequisites installed before setting up and running the Jenkins pipeline:

- Jenkins Server
- Jenkins Pipeline Plugin
- Docker
- AWS CLI
- kubectl
- AWS Credentials in Jenkins
- Iam-authenticator.

In the project structure, a folder named "kubernetes" has been created to organize Kubernetes-related configuration files. Inside this "kubernetes" folder, you will find two essential files: "deployment.yaml" and "service.yaml." These files encapsulate the Kubernetes configurations needed for deploying and exposing the Java Maven application.

This structured approach helps keep Kubernetes-specific configurations separate from other project files, facilitating a cleaner and more organized codebase.

![day14](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/0a74dbec-3a13-4192-b242-fef49ed79943)

# Kubernetes deployment and service file 

To ensure that a new Docker image is created with each pipeline run, we dynamically set the image name in the Kubernetes configuration file. I utilize the environment variable 'IMAGE_NAME' to define the image name, substituting its value during every pipeline execution. This approach ensures that the Kubernetes configuration file always references the latest version of the Docker image, allowing for seamless integration and deployment of the most recent changes.

The imagePullPolicy is typically set to "Always" when you want to ensure that the latest version of the image is always pulled from the registry, even if a local copy exists. This is especially important in scenarios where you want to guarantee consistency across different environments and avoid potential discrepancies.

  
