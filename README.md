# "Project Evolution: Transition to ECR for Build and Push Processes"

Building upon the prior 'Complete-CI-CD-Pipeline-1-Docker' project, the current iteration involves pushing images to Amazon Elastic Container Registry (ECR) for the build and push processes, replacing the Docker usage in the previous project.

## ECR Repository Creation
Begin by creating an Amazon Elastic Container Registry (ECR) repository to serve as a centralized repository for storing Docker images. This repository will facilitate version control and distribution of  containerized applications.

![ola1](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-2-ECR/assets/94229949/377115f3-3039-447a-be37-eceea6233234)

## Jenkins Credentials Setup
In Jenkins, Create the necessary credentials to enable secure access to your ECR repository. This involves configuring authentication details to ensure seamless interaction between Jenkins and ECR during the image build and push processes.

## Create Kubernetes Secret
Generate a Kubernetes secret containing the ECR credentials. This secret will be used by the cluster to authenticate and pull images from the ECR repository.

![ola3](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-2-ECR/assets/94229949/5959cc56-01cc-4cb5-99a4-3a74b7346800)

### Reference Secret in Deployment
In the Kubernetes Deployment, include a reference to the created secret using the imagePullSecrets field. This ensures that the necessary credentials are available for pulling images during deployment.

### ImagePullPolicy Adjustment:
Update the imagePullPolicy in your Kubernetes Deployment to control when the Kubernetes cluster should attempt to pull the specified images. For ECR integration, set imagePullPolicy to "Always" to ensure that the latest image is always pulled from the ECR repository.

![ola6](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-2-ECR/assets/94229949/5ed6924b-a52e-4d23-ac5b-11822ec91a3b)

## Jenkinsfile Modification:
Adjust the Jenkinsfile in the project to accommodate the transition to ECR. Specifically, modify the build and tagging steps to align with the new ECR repository setup. Update any references to Docker commands to reflect the usage of ECR for image storage.

- Establish a global environment block before defining stages in your Jenkinsfile, enabling universal usage across all stages. Create environment variables for Docker repository (DOCKER_REPO) and Docker server (DOCKER_SERVER). These variables are crucial as they are referenced in both the deployment.yaml file and the Jenkinsfile. This approach ensures consistent and centralized management of these variables and can also be easily updated, promoting clarity and efficiency throughout the CI/CD pipeline.

![ola8](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-2-ECR/assets/94229949/f0482891-ae09-4008-914f-d6081d7d13c3)

- Saved ECR username and password credentials in Jenkins for utilization within the Jenkinsfile.

![ola20](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-2-ECR/assets/94229949/aceabc7e-8a2e-49b8-b845-b4be63e49628)

- Edit the build image stage to use the correct credentials and variable .
  
  ![ola17](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-2-ECR/assets/94229949/3d2d2a83-0a41-4616-a99f-9285638e50ca)

## Create a Pipeline Job on Jenkins

Create a Jenkins job on the server and initiate the pipeline using the Git repository URL. This involves configuring a job within Jenkins and triggering the pipeline execution by specifying the repository's URL.

 ![ola9](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-2-ECR/assets/94229949/d49fd572-3745-4f56-89d1-fd3edf9849b0)

 
## Trigger the pipeline

Manually initiate the pipeline execution by clicking the "Build Now" button.

![ola12](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-2-ECR/assets/94229949/af80fbbd-52ac-4a95-bb77-7a08ca4bab0f)

Jenkins modification was applied to the pom.xml file in the GitHub repository.

![ola13](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-2-ECR/assets/94229949/c9743478-a2f7-4048-8acc-ef4640a92ee5)

Pods are running in the EKS cluster

![ola11](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-2-ECR/assets/94229949/47b0625b-b19f-4088-9b86-d5392026a812)

Containers are running an image from the ECR repository.

![ola15](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-2-ECR/assets/94229949/c6119b35-9a53-4a0c-9d8f-d900e5fba5e1)

The image is stored in the ECR 

![ola16](https://github.com/busolagbadero/Complete-CI-CD-Pipeline-2-ECR/assets/94229949/da3cfba7-5fdb-4822-afa1-3e5e175309a9)


 

























  
