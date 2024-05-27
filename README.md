# AWS DevOps

## Project Overview
This project demonstrates the use of various AWS DevOps tools including CodeCommit, CodeBuild, CodeDeploy, and CodePipeline to create a seamless CI/CD workflow. The workflow involves building a repository, setting up a build process, deploying applications, and automating the entire process.

## Workflow

### 1. Build a Repository in CodeCommit
Create a repository in AWS CodeCommit to store your application code.

### 2. Create a User and IAM Role
Create a new user in AWS IAM and assign the necessary permissions for accessing CodeCommit, CodeBuild, CodeDeploy, and other required services.

### 3. Switch to VS Code and Perform Git Commands
Use Visual Studio Code to interact with your CodeCommit repository. Perform all necessary Git commands (clone, commit, push, etc.) to manage your code.

### 4. Set Up CodeBuild
Configure AWS CodeBuild to compile and build your application:
- Create a build project in CodeBuild.
- Configure the source to pull from your CodeCommit repository.
- Set up the build environment and specify the build commands in a `buildspec.yml` file.

### 5. Create a `buildspec.yml` File
Create a `buildspec.yml` file to define the build process and store the build artifacts in an S3 bucket.

```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      python: 3.8
  build:
    commands:
      - echo "Building the application..."
      - # Add your build commands here
artifacts:
  files:
    - '**/*'
  discard-paths: yes
  base-directory: build-output
```

### 6. Save Build Artifacts in S3
Configure the `buildspec.yml` file to save the build output to an S3 bucket.

### 7. Set Up CodeDeploy
Configure AWS CodeDeploy to deploy your application to an EC2 instance:
- Create a deployment application in CodeDeploy.
- Set up the deployment group and specify the target EC2 instances.

### 8. Install the CodeDeploy Agent on EC2

For Amazon Linux 2:
```bash
#!/bin/bash
# This installs the CodeDeploy agent and its prerequisites on Amazon Linux 2.

sudo yum update -y
sudo yum install -y ruby wget

cd /tmp
wget https://aws-codedeploy-us-west-2.s3.us-west-2.amazonaws.com/latest/codedeploy-agent.noarch.rpm
sudo yum install -y codedeploy-agent.noarch.rpm

sudo systemctl start codedeploy-agent
sudo systemctl enable codedeploy-agent
sudo systemctl status codedeploy-agent
```

For Ubuntu:
Follow the instructions in [this guide](https://www.trainwithshubham.com/blog/setting-up-aws-codedeploy-agent-on-ubuntu-ec2).

### 9. Write an `appspec.yml` File
Create an `appspec.yml` file to define the application deployment configuration.

```yaml
version: 0.0
os: linux
files:
  - source: /
    destination: /home/ubuntu/app
hooks:
  AfterInstall:
    - location: scripts/install_dependencies.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 300
      runas: root
```

### 10. Automate the Process with CodePipeline
Set up AWS CodePipeline to automate the entire workflow:
- Create a pipeline in CodePipeline.
- Add the source stage to pull code from CodeCommit.
- Add the build stage to run the CodeBuild project.
- Add the deploy stage to deploy the application using CodeDeploy.

### Troubleshooting
If the EC2 instance is unable to communicate with S3 or CodeDeploy, modify the IAM role to ensure it has the necessary permissions.

### Final Steps
- Create a deployment in CodePipeline.
- Verify the deployment on your EC2 instance.

By following these steps, you will have a fully automated CI/CD pipeline using AWS DevOps tools.
