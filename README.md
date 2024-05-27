# AWS DevOps

## Project Overview
This project demonstrates the use of various AWS DevOps tools including CodeCommit, CodeBuild, CodeDeploy, and CodePipeline to create a seamless CI/CD workflow. The workflow involves building a repository, setting up a build process, deploying applications, and automating the entire process.

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/f06cf38b-495d-43cb-9eca-3c2f65d91575)


## Workflow

### 1. Build a Repository in CodeCommit
Create a repository in AWS CodeCommit to store your application code.

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/22449201-bc2a-4ea4-ac84-f420e03cf879)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/fa726733-9c95-4f6d-9523-8ceb9efc1696)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/6ae84d56-a7a0-4dc6-a3b8-d7562a6225d2)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/20183d82-9c2a-46b3-940a-7fd6578edae2)


### 2. Create a User and IAM Role
Create a new user in AWS IAM and assign the necessary permissions for accessing CodeCommit, CodeBuild, CodeDeploy, and other required services.

### 3. Switch to VS Code and Perform Git Commands
Use Visual Studio Code to interact with your CodeCommit repository. Perform all necessary Git commands (clone, commit, push, etc.) to manage your code.

### 4. Set Up CodeBuild
Configure AWS CodeBuild to compile and build your application:
- Create a build project in CodeBuild.
- Configure the source to pull from your CodeCommit repository.
- Set up the build environment and specify the build commands in a `buildspec.yml` file.

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/b2175f6f-8e77-44ff-8767-fc7caedd8b84)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/09075d40-e785-4a78-9a2f-e58bf0bc47a6)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/d550dad0-6c37-482a-a8ef-e697a70f445b)


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

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/9e00168a-9ba7-4b0b-bca6-d228dc2ec7f7)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/68505612-af83-4b9b-9077-03970d50ce58)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/21142d22-3a7e-4b11-9ab6-99990de60a56)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/3e95073e-b7d1-4c32-a71a-c7d34c0acff1)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/6c8fdb90-d862-4d10-b361-867559042769)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/f08d89c6-88f7-4e02-8695-07f09bc790c0)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/654efc6c-bfb8-4ec4-9bc4-baafc389cd36)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/bb9b9146-4c66-4d1f-9f0e-7ff2a5057633)


### 7. Set Up CodeDeploy
Configure AWS CodeDeploy to deploy your application to an EC2 instance:
- Create a deployment application in CodeDeploy.
- Set up the deployment group and specify the target EC2 instances.

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/3a3c452f-6ff3-449d-9a3f-e235c6dce273)


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

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/d52bf995-e437-4318-998b-2b99ece8c4ee)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/ec21f786-0767-4777-81e2-c16b283d8c05)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/e1f3d881-c105-46d0-80ed-701ad532cc1a)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/f0f0be27-87db-44d6-a0ae-d01ec79ad746)



### 10. Automate the Process with CodePipeline
Set up AWS CodePipeline to automate the entire workflow:
- Create a pipeline in CodePipeline.
- Add the source stage to pull code from CodeCommit.
- Add the build stage to run the CodeBuild project.
- Add the deploy stage to deploy the application using CodeDeploy.

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/318de027-47b7-4e45-9b8c-387a08509525)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/8dbd78b4-c468-4da2-bbaa-41937d466222)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/92da82ad-0e70-4cd7-b229-c09183b719b2)

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/8f72a1c5-6ed4-49d2-ad1b-49bf02fbadbc)


### Troubleshooting
If the EC2 instance is unable to communicate with S3 or CodeDeploy, modify the IAM role to ensure it has the necessary permissions.

### Final Steps
- Create a deployment in CodePipeline.
- Verify the deployment on your EC2 instance.

![image](https://github.com/Aditi55Pathak/Devops/assets/80877301/06de1c34-2cac-48d9-929e-0b332356e183)

By following these steps, you will have a fully automated CI/CD pipeline using AWS DevOps tools.
