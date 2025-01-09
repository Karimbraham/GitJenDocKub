# Jenkins CI/CD Pipeline with Docker and Kubernetes

This project demonstrates a CI/CD pipeline using **Jenkins**, **Docker**, and **Kubernetes**. The pipeline performs the following steps:

1. **Clean Workspace**: Cleans the Jenkins workspace to ensure a fresh start for each build.
2. **Checkout the Code**: Retrieves the source code from the `main` branch of the Git repository.
3. **Build Application**: Builds the application using Maven.
4. **Test Application**: Runs unit tests to validate the application.
5. **Build and Push Docker Image**: Builds a Docker image for the application and pushes it to DockerHub.
6. **Deploy to Kubernetes**: Deploys the application to a Kubernetes cluster using the specified Kubernetes manifests.

---

## Prerequisites

To run this pipeline, ensure the following are set up:

- **Jenkins**:
  - Installed and running with necessary plugins:
    - Docker Pipeline
    - Kubernetes
    - Git
    - Pipeline
  - Jenkins user has access to Docker and Kubernetes CLIs.
- **Docker**:
  - Installed and running.
  - Access to a DockerHub account with valid credentials.
- **Kubernetes**:
  - A running Kubernetes cluster (e.g., Minikube, GKE, EKS, AKS).
  - `kubectl` configured to interact with the cluster.
- **DockerHub**:
  - A DockerHub account for storing Docker images.
  - Credentials stored in Jenkins as a secret (`DOCKER_PASS`).
- **Git Repository**:
  - Source code hosted in a version control system (e.g., GitHub).

---

## Jenkins Pipeline Configuration

### Jenkinsfile

The pipeline is defined in the `Jenkinsfile`. Below is the summary of the pipeline's stages:

1. **Clean Workspace (CWS)**:
   - Cleans the Jenkins workspace to remove any previous build artifacts.

2. **Checkout Code**:
   - Pulls the source code from the `main` branch of the GitHub repository:
     ```groovy
     git branch: 'main', credentialsId: 'github', url: 'https://github.com/Karimbraham/GitJenDocKub'
     ```

3. **Build Application**:
   - Uses Maven to clean and package the application:
     ```bash
     mvn clean package
     ```

4. **Test Application**:
   - Runs unit tests using Maven:
     ```bash
     mvn test
     ```

5. **Build and Push Docker Image**:
   - Builds a Docker image using the application code.
   - Pushes the image to DockerHub with the following tags:
     - `${IMAGE_TAG}` (e.g., `3.0.0-1`)
     - `latest`
   - Docker credentials (`DOCKER_PASS`) are used for authentication.

6. **Deploy to Kubernetes**:
   - Applies Kubernetes manifests from the `kubernetes-manifests/` directory to deploy the application.

---

## Setting Up Jenkins

1. **Install Jenkins Plugins**:
   - Navigate to `Manage Jenkins > Manage Plugins`.
   - Install the required plugins: Docker Pipeline, Kubernetes, Git, Pipeline.

2. **Add DockerHub Credentials**:
   - Navigate to `Manage Jenkins > Manage Credentials`.
   - Add your DockerHub username and password as a secret with an ID (e.g., `DOCKER_PASS`).

3. **Add GitHub Credentials**:
   - Add your GitHub credentials (e.g., Personal Access Token) to Jenkins with an ID (e.g., `github`).

4. **Create a Pipeline Job**:
   - In Jenkins, create a new job of type "Pipeline".
   - Point the job to your `Jenkinsfile`.

5. **Configure Kubernetes Credentials** (if needed):
   - Set up service account credentials in Jenkins to interact with your Kubernetes cluster.

---
 

## Usage

1. Push your application code and `Jenkinsfile` to the GitHub repository.
2. Configure the pipeline in Jenkins.
3. Trigger the pipeline build in Jenkins.
4. Monitor the pipeline's progress in the Jenkins UI.
5. Verify the deployment in the Kubernetes cluster using:

   ```bash
   kubectl get pods
   kubectl get services
   ```

---

## License

This project is licensed under the [GPL-3.0 license](LICENSE).
 
