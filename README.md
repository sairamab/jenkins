# Repo for Jenkins Practice and Projects

## Introduction

Jenkins is an open-source tool widely used for Continuous Integration (CI) and Continuous Deployment (CD), which helps in automating builds, tests, and deployments.

### Why Jenkins?
- **Easy Installation**
- **Scalable Architecture** (Runs builds on multiple agents)
- **Extensive Plugin Support** (More than 1800 plugins!)
- **Supports Declarative and Scripted Pipelines**
- **Integrates with Various Tools** (Many integrations done through plugins)

### Jenkins Architecture
- **Jenkins Controller (Master)**: Handles UI, job scheduling, plugin management, workflow triggering, and result display.
- **Jenkins Agents (Slaves)**: Responsible for executing jobs assigned by the controller.
- **Job**: A task that needs to be executed.

Jenkins allows task creation through multiple methods, with pipelines being the most widely used approach:
- **Declarative Pipelines**: Structured, readable format with easier maintenance.
- **Scripted Pipelines**: More complex but highly customizable.

## Installation

Jenkins can be installed in multiple ways, with Java as a prerequisite since it is a Java-based application. The installation steps can be referred to in the official documentation.

### Jenkins Installation on Windows using Docker
1. Install Docker (Docker Desktop).
2. Run the following command to start Jenkins:
   ```sh
   docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
   ```
3. Access Jenkins through [http://localhost:8080](http://localhost:8080).
4. Connect to the working environment with:
   ```sh
   docker exec -it -u root <container-id> /bin/bash
   ```
   - Obtain `<container-id>` by checking the running Jenkins process using `docker ps`.

### Master-Slave Architecture
In the above setup, Jenkins runs in a single container without a separate agent. By default, it executes jobs on the same instance unless an agent is configured separately.

To set up a master-agent architecture:
- Start a separate container for the agent.
- Establish a connection between the agent and master using:
  - **SSH** (for Linux agents)
  - **JNLP jar** (for Windows agents)

## Project

A simple `Jenkinsfile` that performs the CI process:
1. Cleans up the current working directory.
2. Pulls code from a public repository with a Maven project.
3. Builds using Maven.
4. Runs tests.

## Troubleshooting

Before running the `Jenkinsfile` successfully, some builds failed. Follow these steps to avoid common issues:

### Maven Version Issue
**Error:**
```
Detected Maven Version: 3.8.7 is not in the allowed range [3.9.2,)
```
- The container runs Maven 3.8.7, but the requirement is at least 3.9.2.
- Updating Maven inside the container was unsuccessful, so we specified the required version in the `Jenkinsfile`:

  ```groovy
  tools {
      maven 'Maven-3.9.2' // Uses the Maven installed via Jenkins
  }
  ```

- Set the Maven version in Jenkins UI:
  - Navigate to **Dashboard → Manage Jenkins → Tools → Maven Installations**
  - Click **Add Maven**
  - Set the name to `Maven-3.9.2` (must match `Jenkinsfile` entry)
  - Select Maven version **3.9.2**

This ensures that the required Maven version is met.
