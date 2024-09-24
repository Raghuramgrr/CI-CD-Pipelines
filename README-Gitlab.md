# CI-CD-Pipelines
Gitlab &amp; Jenkins
# CI/CD Pipeline for Cloud-Native Applications

This repository contains a comprehensive CI/CD pipeline for cloud-native applications using Docker and Kubernetes, implemented in Jenkins. The pipeline includes stages for building, testing, deploying, and cleaning up resources.

## Pipeline Overview

The pipeline consists of the following stages:

1. **Build**: 
   - Builds the Docker image from the `dockerfiles` directory.
   - Logs into the Docker registry using environment variables for the username and password.
   - Pushes the Docker image to the registry.
   - Triggers on the `main` and `develop` branches.

2. **Test**: 
   - Runs unit tests using Node.js.
   - Installs dependencies and executes tests using `npm`.
   - Only runs for the `main` and `develop` branches.

3. **Deploy to Development**: 
   - Applies Kubernetes manifests for the development environment.
   - Deploys the application for testing in a development setting.

4. **Deploy to Production**: 
   - Triggers only on the `main` branch, deploying to the production environment.

5. **Cleanup**: 
   - Manually triggered to remove dangling Docker images, preventing clutter.
   - Ensures that only users with access can run this stage.

## Notifications

- Sends notifications to a Slack channel upon successful deployment or failure.
- Uses a Slack webhook for notifications and runs only on the `main` branch when the deployment is successful.

## Customization

### Docker Registry
- Update the `DOCKER_REGISTRY` variable in the pipeline script with your actual Docker registry URL.

### Testing Framework
- Modify the test stage as per the framework you are using (e.g., Maven for Java, pytest for Python, etc.).

### Slack Webhook
- Set up a Slack webhook and add it as a Jenkins credential for secure access under the variable `SLACK_WEBHOOK_URL`.

## Conclusion

This pipeline structure provides a robust CI/CD workflow that incorporates building, testing, deploying, cleaning up, and notifying stakeholders about deployment statuses. You can further extend this pipeline by adding more stages or features as per your project requirements.

