# AWS-Ci-cd-Pipeline
⭐ PROJECT: AWS CI/CD Pipeline for Deploying a Web Application to EC2

This is the simplest real DevOps project using ONLY AWS:

✔ CodeCommit
✔ CodeBuild
✔ CodeDeploy
✔ S3
✔ EC2
✔ IAM
✔ CloudWatch Logs
<img width="1242" height="609" alt="image" src="https://github.com/user-attachments/assets/5a40bf43-019a-448c-9bfe-cc9b65d13f56" />

<img width="800" height="333" alt="image" src="https://github.com/user-attachments/assets/a9561339-4f33-44ad-8401-bd8b04123e7f" />

Flow:
Developer pushes code → CodeCommit → CodePipeline → CodeBuild → S3 Artifact → CodeDeploy → EC2

⭐ 2. Project Use Case 

“I created an automated CI/CD pipeline on AWS that deploys a simple web application to an EC2 instance. I used CodeCommit for source control, CodeBuild for building the application, and CodeDeploy for deployment. CodePipeline connected all these stages.”

⭐ 3. Step-by-Step Project Explanation (With Files & Why Each Step)
STEP 1 — Create a Web App Repository in CodeCommit

Why?
Because we need a source code repo for CI/CD.

Files inside CodeCommit:
index.html
app.js (optional)
buildspec.yml
appspec.yml
scripts/
    install_dependencies.sh
    start_server.sh


STEP 2 — Launch an EC2 Instance

Why?
This EC2 is your target server where CodeDeploy will deploy your application.

Important setups:

Install CodeDeploy Agent

Install Apache / Nginx

Attach IAM Role (EC2Role with S3 + CodeDeploy access)

STEP 3 — Create S3 Bucket

Why?
To store build artifacts from CodeBuild (zip files).

STEP 4 — Create CodeBuild Project

Why?
CodeBuild compiles/build/tests your application.

You must create this file: buildspec.yml

This tells CodeBuild what to do.

Example:
version: 0.2
phases:
  install:
    commands:
      - echo Installing dependencies
  build:
    commands:
      - echo Build Completed
artifacts:
  files:
    - '**/*'
<img width="451" height="333" alt="image" src="https://github.com/user-attachments/assets/970ed886-0192-4e78-a81a-b01cb1c09a5c" />


Why this file?
→ CodeBuild reads it to know how to build and what to output.

STEP 5 — Create CodeDeploy Application

Why?
CodeDeploy deploys your application from S3 → EC2.

REQUIRED FILE: appspec.yml

This tells CodeDeploy what to do during deployment.

Example:

version: 0.0
os: linux
files:
  - source: /
    destination: /var/www/html/

hooks:
  BeforeInstall:
    - location: scripts/install_dependencies.sh
      timeout: 300
  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 300
<img width="621" height="403" alt="image" src="https://github.com/user-attachments/assets/adcf850e-959d-4715-805c-cbaf6e210a1d" />


Why?
→ This controls your entire deployment process.

STEP 6 — Add Deployment Scripts

Inside /scripts folder:

install_dependencies.sh
#!/bin/bash
sudo yum install -y httpd

start_server.sh
#!/bin/bash
sudo systemctl start httpd


Why?
→ These scripts automate installation & starting of the web server.

STEP 7 — Create CodePipeline

Why?
CodePipeline connects all the stages:

Source → CodeCommit

Build → CodeBuild

Deploy → CodeDeploy

Pipeline will run automatically on every code push.


🔥 1–2 Minutes Perfect Explanation

“In my project, I implemented a full CI/CD pipeline using only AWS services.
I stored the application code in CodeCommit, and every time code was pushed, CodePipeline automatically triggered the pipeline.
CodeBuild used a buildspec.yml file to package the application and store the artifact in S3.
Then CodeDeploy used an appspec.yml file and deployment scripts like install_dependencies.sh and start_server.sh to deploy the application on an EC2 instance.
The entire process—from code push to deployment—was automated with no manual steps.
This improved deployment speed, reduced human errors, and ensured consistent deployments.”
