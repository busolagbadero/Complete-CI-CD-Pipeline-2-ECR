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


# Maven Tool:

To ensure consistency in the Jenkins environment, configured Maven as 'maven-b' using Jenkins' global tool settings. This standardization simplifies project builds, allowing seamless management and updates of Maven installations across different Jenkins agents.

![day2](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/c21335b3-22ef-479f-8dac-9f649d37117c)

# Version Increment

The pipeline has a stage named "increment Version". 

- sh 'mvn build-helper:parse-version versions:set ...': Executes a Maven command to parse the version from the pom.xml file and increment it. The new version is then committed.

- def matcher = readFile('pom.xml') =~ '<version>(.+)</version>': Reads the contents of the pom.xml file and extracts the version using a regular expression.

- def version = matcher[0][1]: Assigns the extracted version to the variable version.

- env.IMAGE_NAME = "$version-$BUILD_NUMBER": Constructs an environment variable IMAGE_NAME by combining the extracted version and the Jenkins build number.

 The pipeline increments the version in the pom.xml file, extracts the new version and constructs an image name using the version and Jenkins build number. This image name is then stored in the environment variable IMAGE_NAME.

![day19](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/c9a4d0e3-ef88-47b0-abe3-5197fc063e5a)


In the project structure, a folder named "kubernetes" has been created to organize Kubernetes-related configuration files. Inside this "kubernetes" folder, you will find two essential files: "deployment.yaml" and "service.yaml." These files encapsulate the Kubernetes configurations needed for deploying and exposing the Java Maven application.

This structured approach helps keep Kubernetes-specific configurations separate from other project files, facilitating a cleaner and more organized codebase.

![day14](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/0a74dbec-3a13-4192-b242-fef49ed79943)

# Kubernetes deployment and service file 

To ensure that a new Docker image is created with each pipeline run, I dynamically set the image name in the Kubernetes configuration file and also utilize the environment variable 'IMAGE_NAME' to define the image name, substituting its value during every pipeline execution. This approach ensures that the Kubernetes configuration file always references the latest Docker image version, allowing for seamless integration and deployment of the most recent changes.

The imagePullPolicy is typically set to "Always" to ensure that the latest version of the image is always pulled from the registry, even if a local copy exists. This is important as it  guarantees consistency across different environments and avoids discrepancies.

I've replaced the repeated occurrence of "java_maven_app" with a variable, making it easier to manage and modify consistently across the deployment and service files. The variable "$APP_NAME" now serves as a placeholder, providing a centralized point for updates and alterations.

![day15](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/4b4ebb18-c403-4cdb-9688-d8472c43e2c1)

![day16](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/84ae5d54-42ee-4f82-a7f5-db0a4cb0cb71)

## Add AWS Credentials on Jenkins

Incorporating AWS credentials into Jenkins is essential for establishing secure and authenticated communication between Jenkins and AWS services. This step ensures that Jenkins has the necessary access to AWS resources, facilitating the deployment process. By providing AWS access key and secret access key, Jenkins gains the authorization to interact with the specified AWS account, enhancing the overall security and functionality of the CI/CD pipeline.

![eks9](https://github.com/busolagbadero/Deploy-to-EKS-cluster-from-Jenkins/assets/94229949/1fb22c6d-c254-4a0a-8263-53d21d20718e)

![eks10](https://github.com/busolagbadero/Deploy-to-EKS-cluster-from-Jenkins/assets/94229949/cd222eec-4b38-432b-b3db-f9bb80974d61)

# Set Environment Variable in Jenkinsfile

Environment variables can be configured in the global block, just before 'stages', making them accessible to all stages. Alternatively, they can be defined within a specific stage, such as the 'Deploy_env' stage in the Jenkinsfile, limiting their scope to that particular stage.

To substitute values from the Jenkinsfile into a YAML file, use the envsubst command tool. This tool replaces environment variables in the YAML file with their corresponding values defined in the Jenkinsfile, ensuring dynamic configuration during the pipeline execution. The envsubst command needs to be installed on the system where the Jenkins pipeline is running. command to install tool is 'apt-get install gettext-base'

![day3](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/81da94c3-44ab-46ce-bc78-2ec23d96a7cd)

![day4](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/6b6d202f-0f6e-4336-a80f-24b78783d099)

![day17](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/0554df77-ac4d-49f3-a820-7541bc48d900)



# Create Secret for Dockerhub Credentials

Creating a secret key to enable Kubernetes to pull images from a private Docker repository is a crucial step for securing access.

![day20](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/0884a17e-ce8d-42d5-91d3-61835bec813a)

![day5](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/9d55937b-61e2-49ea-aa5d-5657864cd409)

Update Kubernetes Deployment in Kubernetes Deployment.yaml  file, reference the created secret in the imagePullSecrets section:

![day18](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/43e42f1b-e983-4aee-b3d9-caa3c4e88f54)

This ensures that Kubernetes can authenticate with the private Docker repository using the specified secret.

# GIT Commit 

To enable the Jenkins pipeline to commit changes to a Git repository, generate an SSH key pair using `ssh-keygen`. The public key should be added to the Git repository settings under SSH keys, while the private key needs to be stored securely in Jenkins server credentials. This setup allows Jenkins to authenticate and perform Git commits seamlessly.

![day7](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/56545e3a-5c0f-437a-89ac-d7930f1a6dee)

![day8](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/ec1aef50-77b0-4f88-a67c-4f87023cf68a)

# Create a Pipeline Job

Create a Jenkins job on the server and initiate the pipeline using the Git repository URL. This involves configuring a job within Jenkins and triggering the pipeline execution by specifying the repository's URL.

![day9](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/85a23840-8a8c-48c7-ac7b-7cbb986b1cc6)

Trigger the pipeline

![day10](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/e0d4d55d-a444-4322-a648-4a60cd7bdcb3)

# Result

![day11](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/c0781ab0-7e37-4e6f-b5e9-d78b88002874)

![day12](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/a0b043da-0cf4-44b8-9da1-5174c2ff1848)

![day13](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-1-/assets/94229949/922cba90-0afa-456e-be2f-e82baca2687b)

The image name, as well as the deployment and service names, are dynamically replaced during the configuration process.
















  
