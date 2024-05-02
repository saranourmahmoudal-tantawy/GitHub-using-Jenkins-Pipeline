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

# Jenkins Setup on AWS EC2

### Launch an AWS EC2 Instance:
1. Log in to the AWS Management Console.
2. Navigate to the EC2 dashboard.
3. Launch a new EC2 instance with Amazon Linux 2 AMI.

## Install Jenkins on EC2:

### Connect to Your EC2 Instance Using SSH:
1. Open a terminal or command prompt.
2. Use SSH to connect to your EC2 instance:
   ```bash
   ssh -i <path_to_your_key_pair.pem> ec2-user@<your_ec2_public_ip>
   ```
3.install Jenkins
  ```bash
   sudo yum install java-17-amazon-corretto-devel -y
   sudo wget -O /etc/yum.repos.d/jenkins.repo \
   https://pkg.jenkins.io/redhat-stable/jenkins.repo
   sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
   sudo yum upgrade
   sudo yum install jenkins
   ```


