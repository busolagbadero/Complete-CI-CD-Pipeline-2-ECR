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

To ensure that a new Docker image is created with each pipeline run, I dynamically set the image name in the Kubernetes configuration file and also utilize the environment variable 'IMAGE_NAME' to define the image name, substituting its value during every pipeline execution. This approach ensures that the Kubernetes configuration file always references the latest Docker image version, allowing for seamless integration and deployment of the most recent changes.

The imagePullPolicy is typically set to "Always" to ensure that the latest version of the image is always pulled from the registry, even if a local copy exists. This is important as it  guarantees consistency across different environments and avoids discrepancies.

I've replaced the repeated occurrence of "java_maven_app" with a variable, making it easier to manage and modify consistently across the deployment and service files. The variable "$APP_NAME" now serves as a placeholder, providing a centralized point for updates and alterations.

![day15](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/4b4ebb18-c403-4cdb-9688-d8472c43e2c1)

![day16](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/84ae5d54-42ee-4f82-a7f5-db0a4cb0cb71)


# Set Environment Variable in Jenkinsfile

Environment variables can be configured in the global block, just before the stages, making them accessible to all stages. Alternatively, they can be defined within a specific stage, such as the 'Deploy_env' stage in the Jenkinsfile, limiting their scope to that particular stage.

To substitute values from the Jenkinsfile into a YAML file, use the envsubst command tool. This tool replaces environment variables in the YAML file with their corresponding values defined in the Jenkinsfile, ensuring dynamic configuration during the pipeline execution. The envsubst command needs to be installed on the system where the Jenkins pipeline is running. command to install tool is 'apt-get install gettext-base'

![day3](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/81da94c3-44ab-46ce-bc78-2ec23d96a7cd)

![day4](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/6b6d202f-0f6e-4336-a80f-24b78783d099)

![day17](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/0554df77-ac4d-49f3-a820-7541bc48d900)




  
