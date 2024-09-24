# CI/CD Pipeline for Jenkins

This repository contains a comprehensive CI/CD pipeline for cloud-native applications using Docker and Kubernetes, implemented in Jenkins. This pipeline streamlines the process of building, testing, deploying, and maintaining your applications while keeping stakeholders informed.

## Pipeline Overview

### Environment Variables
- **DOCKER_REGISTRY**: Specifies your Docker registry URL.
- **DOCKER_IMAGE**: Constructs the full image name for easier reference.
- **SLACK_WEBHOOK_URL**: Uses `credentials()` to securely access the Slack webhook URL.

### Stages
1. **Build**:
   - Builds the Docker image from the `dockerfiles` directory.
   - Logs into the Docker registry using environment variables for the username and password.
   - Pushes the Docker image to the registry.

2. **Test**:
   - Includes parallel stages for unit tests and integration tests, allowing them to run simultaneously.
   - Modify the testing commands based on the framework you're using (e.g., Maven for Java, pytest for Python).

3. **Deploy to Development**:
   - Applies Kubernetes manifests for the development environment.

4. **Deploy to Production**:
   - Only executed on the `main` branch, deploying to the production environment.

5. **Cleanup**:
   - Removes dangling Docker images to free up space.

### Post Actions
- Sends notifications to a Slack channel upon successful completion or failure of the pipeline.
- Uses `curl` to post messages to Slack, providing instant feedback on the deployment status.

## Customization
- **Docker Registry**: Update the `DOCKER_REGISTRY` variable in the pipeline script with your actual Docker registry URL.
- **Kubernetes Manifests**: Ensure the paths to your Kubernetes manifests are correct and specific to your environments (dev and prod).
- **Slack Integration**: Set up a Slack webhook and add it as a Jenkins credential for secure access under the variable `SLACK_WEBHOOK_URL`.

## Conclusion
This pipeline provides a comprehensive CI/CD solution, enabling you to build, test, deploy, and maintain your cloud-native applications effectively while keeping stakeholders informed. You can further extend this pipeline by adding more stages or features as per your project requirements.

