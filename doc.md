## Experiment 5: Create a Docker Image for an Application and Run the Application Using Docker Image

**Date:** 06-04-2026
**Page No:** 11–12

---

## Aim

To create a Docker image for a Python application, run the application inside a Docker container, and push the image to Docker Hub.

---

## Requirements

* Docker Desktop
* VS Code
* Python
* Docker Hub account

---

## Procedure

### Step 1: Create Project Folder

Create a folder named `dockerimage` inside another folder called `Devops`.

---

### Step 2: Create Required Files

Inside the `dockerimage` folder, create the following files:

* `sample.py`
* `Dockerfile`

### sample.py

```python
print("Hello World")
```

---

### Step 3: Write Dockerfile

```dockerfile
FROM python

WORKDIR /app

COPY . /app

CMD ["python", "sample.py"]
```

---

### Step 4: Execute Commands

Open terminal inside the `dockerimage` directory and execute:

```bash
py sample.py
```

```bash
docker build -t test.1 .
```

```bash
docker run --name cont1 test.1
```

* `test.1` → Image name
* `cont1` → Container name

---

### Step 5: Verify Docker Image

Open Docker Desktop and verify whether the image `test.1` is created successfully.

---

### Step 6: Copy Image Tag

Copy the image tag from Docker Desktop.

Example:

```text
test.1:latest
```

---

### Step 7: Push Image to Docker Hub

Execute the following commands:

```bash
docker image tag <copied_image_tag> <username>/test.1
```

```bash
docker push <username>/test.1:latest
```

---

### Step 8: Verify on Docker Hub

Open Docker Hub and verify that the image has been pushed successfully.

---

## Output

```text
Hello World
```

---

## Result

Successfully created a Docker image, executed the container, and pushed the image to Docker Hub.

---

# Experiment 6: Create and Configure Jenkins Pipeline for Workflow and Build of an Application

**Page No:** 13–15

---

## Aim

To create and configure Jenkins pipeline jobs for application workflow automation and Docker image build process.

---

## Requirements

* GitHub Account
* Jenkins
* Docker Desktop
* VS Code
* Git Bash

---

## Procedure

### Step 1: Create GitHub Repository

Create a repository named `dockerprog`.

Clone the repository using:

```bash
git clone "<your_repo_url>"
```

---

### Step 2: Create Application Files

Inside the `dockerprog` folder create:

* `Dockerfile`
* `sample.py`

### sample.py

```python
print("Hello World")
```

### Dockerfile

```dockerfile
FROM python

WORKDIR /app

COPY . /app

CMD ["python", "sample.py"]
```

---

### Step 3: Commit and Push Changes

```bash
git add .
git commit -m "Initial Commit"
git push
```

---

### Step 4: Build Docker Image and Container

```bash
docker build -t test.2 .
```

```bash
docker run --name cont2 test.2
```

Check available images:

```bash
docker images
```

Tag the image:

```bash
docker tag <image_id> ayusht05/test.2
```

Push image:

```bash
docker push ayusht05/test.2:latest
```

---

### Step 5: Install Jenkins Plugins

Navigate to:

```text
Manage Jenkins → Plugins
```

Install/update:

* CloudBees Docker Custom Build Environment
* Docker
* Docker Pipeline
* Docker Compose Build Step

---

### Step 6: Create Jenkins Pipeline Job

Create a new Jenkins job named:

```text
Doc2
```

Select:

```text
Pipeline
```

---

### Step 7: Add Pipeline Script

```groovy
pipeline {

    agent any

    environment {
        dockerImage = "ayusht05/test.2"
        registry = "ayusht05/test.2"
        registryCredential = 'jenkin_docker-token'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/AyushThanthri/dockerprog.git'
                    ]]
                )
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build(registry)
                }
            }
        }
    }
}
```

if this doesnt work, then use this 

```groovy
pipeline {
    agent any

    environment {
        dockerImage = "ayusht05/test_3"
        registry = "ayusht05/test_3"
        registryCredential = 'jenkin_docker-token'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/AyushT05/DockerProg.git']]
                )
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${dockerImage}")
                }
            }
        }

    }
}
```
---

### Step 8: Execute Pipeline

* Click **Save**
* Click **Build Now**

Verify successful pipeline execution.

---

## Result

Successfully created and configured a Jenkins pipeline for Docker image build automation.

---

# Experiment 7: Integrate Communication Channel with Jenkins for Status of Project and Enable Email Notification

**Date:** 04-05-2026
**Page No:** 16–18

---

## Aim

To integrate Slack with Jenkins for build status notifications and project communication.

---

## Requirements

* Slack Workspace
* Jenkins
* Slack Notification Plugin

---

## Procedure

### Step 1: Install Slack

Download Slack:

[Slack Download Page](https://slack.com/intl/en-in/downloads/windows?utm_source=chatgpt.com)

* Login to Slack
* Join/create workspace
* Create a public channel

Example:

```text
devops-group-channel
```

---

### Step 2: Install Jenkins Plugin

Navigate to:

```text
Manage Jenkins → Plugins
```

Install:

```text
Slack Notification Plugin
```

---

### Step 3: Configure Jenkins System

Navigate to:

```text
Manage Jenkins → System
```

Under Slack section:

* Enter Workspace Name
* Add Credentials

---

### Step 4: Configure Slack Integration

Open:

```text
https://my.slack.com/services/new/jenkins-ci
```

* Select the channel
* Click **Add Jenkins CI Integration**

Copy the Integration Token/Credential ID.

---

### Step 5: Add Credentials in Jenkins

Navigate to:

```text
Manage Jenkins → Credentials
```

Add:

```text
Secret Text
```

Paste the copied credential ID.

---

### Step 6: Configure Job Notifications

Inside Jenkins job:

* Open Build Environment
* Add build step

Example Windows Batch Command:

```bash
echo Build Started
```

Enable notification options:

* Notify Build Start
* Notify Build Success
* Notify Every Failure

Save and run the build.

---

### Step 7: Verify Slack Notification

Open the Slack channel and verify Jenkins build notifications.

---

## Output

```text
#1 Started by user anonymous
#1 Success after 1 sec
```

---

## Result

Successfully integrated Slack communication channel with Jenkins and enabled build notifications.

---
