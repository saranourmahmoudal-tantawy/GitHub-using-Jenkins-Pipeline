# GitHub-using-Jenkins-Pipeline
## Overview
This project demonstrates the integration of Jenkins with a GitHub repository to automate the build and test processes using a Jenkinsfile.

## Tools Needed
1- **AWS EC2:** Virtual server hosting platform for deploying Jenkins and running CI/CD pipelines.

2- **Jenkins:** Continuous Integration (CI) server for automating the build and test processes.

3- **GitHub account:** Version control repository hosting platform for storing your codebase.

4- **Git installed on the Jenkins server:** Version control system required for Jenkins to pull code from the GitHub repository.

5- **Git Flow:** A branching model for Git that helps manage large projects with multiple features and releases.

# Setup Instructions

## GitHub Repository Setup

1. **Create a new GitHub repository:**
    - Log in to your GitHub account.
    - Click on the "+" icon in the top-right corner and select "New repository".
    - Choose a name for your repository, add a description, and select "Initialize this repository with a README".
    - Click "Create repository".

2. **Ensure the repository contains at least two branches:**
    - GitHub typically initializes with the main branch.
    - Create a develop branch using the following commands:
    ```bash
    git checkout -b develop
    git push origin develop
    ```

### Install Git Flow tools on your local machine:

```bash
git clone -b feature/https-remote https://github.com/jgonggrijp/gitflow.git
curl -OL https://raw.github.com/jgonggrijp/gitflow/develop/contrib/gitflow-installer.sh
chmod +x gitflow-installer.sh
sudo ./gitflow-installer.sh
```
### Initialize Git Flow in your repository with default branch names:

```bash
git flow init
```

### Add a Jenkinsfile to your repository:

1. Create a file named Jenkinsfile in the root directory of your repository.

2. Use the following template for a basic pipeline:

```groovy
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
```

## Jenkins Setup on AWS EC2

### Launch an AWS EC2 Instance:
1. Log in to the AWS Management Console.
2. Navigate to the EC2 dashboard.
3. Launch a new EC2 instance with Amazon Linux 2 AMI.

Note :

Add inbound rule in the security group of the EC2 instance:

Type: Custom TCP

Port Range: 8080

source: 0.0.0.0/0

### Install Jenkins on EC2:

**Connect to Your EC2 Instance Using SSH**
1. Open a terminal or command prompt.
2. Use SSH to connect to your EC2 instance:
   ```bash
   ssh -i <path_to_your_key_pair.pem> ec2-user@<your_ec2_public_ip>
   ```
3. install Jenkins
   ```bash
   sudo yum install java-17-amazon-corretto-devel -y
   sudo wget -O /etc/yum.repos.d/jenkins.repo \
   https://pkg.jenkins.io/redhat-stable/jenkins.repo
   sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
   sudo yum upgrade
   sudo yum install jenkins
   ```
4. Start Jenkins service on EC2
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl start jenkins
    ```

## Manage Jenkins:

1. **Open Jenkins Dashboard:**
   - Open a web browser.
   - Enter the public IP address of your EC2 instance followed by port 8080 (e.g., http://<EC2_public_IP>:8080).
   - Press Enter to navigate to the Jenkins dashboard.

2. **Unlock Jenkins and Set Up Admin User:**
   - Follow the on-screen instructions to unlock Jenkins.
   - Set up an admin user by providing the necessary details.

## Configure Jenkins to Connect to GitHub:

1. **Install GitHub Integration Plugin:**
   - Go to "Manage Jenkins" > "Manage Plugins" > "Available".
   - Search for "GitHub Integration Plugin" and install it.

2. **Set Up GitHub Credentials:**
   - Go to "Manage Jenkins" > "Manage Credentials".
   - Click on "Global credentials (unrestricted)" and then "Add Credentials".
   - Choose "Username with password" as the kind.
   - Enter your GitHub username and token.
   - Save the credentials.

## Create a New Pipeline Job:

1. **Create a New Item:**
   - From the Jenkins dashboard, click on "New Item".
   - Enter a name for your pipeline (e.g., "GitHub Pipeline").
   - Choose "Pipeline" as the type.
   - Click "OK" to proceed.

2. **Configure Pipeline Script from SCM:**
   - In the pipeline configuration, select "Pipeline script from SCM".
   - Choose "Git" as the SCM.
   - Enter the repository URL and credentials.
   - Specify the branch to build (e.g., */develop).
   - Save the configuration.

## Implement Webhooks for Continuous Integration:

1. **Go to GitHub Repository Settings:**
   - Open your repository settings on GitHub.

2. **Add a New Webhook:**
   - Navigate to the "Webhooks" section.

3. **Configure Webhook Settings:**
   - Click on "Add webhook" or "Create webhook".
   - Set the Payload URL to http://<your-jenkins-url>/github-webhook/.
   - Choose "application/json" as the Content type.
   - Select "Just the push event" for the events to trigger Jenkins builds.
   - Ensure that the webhook is active.

4. **Save Webhook Configuration:**
   - Click on "Add webhook" or "Create webhook" to save the configuration.


## Testing and Validation:

1. **Push Changes to the Develop Branch:**
   - Make changes to your code and push them to the develop branch of your GitHub repository.

2. **Verify Jenkins Automation:**
   - Check Jenkins to ensure that it automatically triggers a build upon receiving the webhook payload.
   - Monitor Jenkins console output to confirm the successful execution of the defined pipeline stages in the Jenkinsfile.

3. **Ensure Successful Execution:**
   - Validate that the build, test, and deploy stages are executed without errors.
   - Review the Jenkins console output for any error messages or warnings.

