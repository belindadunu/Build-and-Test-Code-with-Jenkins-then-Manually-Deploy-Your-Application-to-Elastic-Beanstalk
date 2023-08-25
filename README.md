# Jenkins Pipeline for Python App Deployment

## Overview

In this project, we're going to implement continuous integration and delivery (CI/CD) for a Python URL shortener application using Jenkins pipelines. This provides several key benefits:

- Automating builds and tests will allow faster development of new features for the shortener app.
- Running automated tests in the pipeline will help maintain quality and catch bugs in the shortener code early.
- Using Jenkins for CI/CD provides a reproducible pipeline for this Python application as it grows.
- Integrating with GitHub enables automatically building on every code change.

We are deploying the shortener app to AWS Elastic Beanstalk to get the benefits of auto-scaling, load balancing, and managed infrastructure. Elastic Beanstalk will handle provisioning servers, scaling capacity, configuring security groups, and load balancing requests automatically as the shortener app usage grows.

## Requirements
To complete this project, we'll need:

- AWS Account
- Elastic Beanstalk
- Jenkins
- GitHub
- EC2 Instance

## Steps

## Set Up Your Virtual Environment:

First, we need to set up a virtual environment to run Jenkins and our pipeline.

1. Create a free [AWS account](https://aws.amazon.com/) if you don't already have one.
2. Use the AWS Console to launch an EC2 instance running the Ubuntu Server OS.
3. SSH into the EC2 instance to configure it.

### Install Jenkins

Next, we'll install Jenkins on our EC2 instance:

1. Add the Jenkins apt repository key:

`$ curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null` 

3. Add the Debian package repository address:

`$ echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null` 

5. Update apt and install Jenkins:

`$ sudo apt-get update` 
`$ sudo apt-get install jenkins` 

7. Start Jenkins service:

`$ sudo systemctl start jenkins` 

8. Access the Jenkins web UI at `http://your_server:8080` and unlock it.

9. Install suggested plugins to get started. In this project, the Jenkins plugin _"Pipeline Utility Steps"
_

### Create Pipeline

Now we can create a pipeline in Jenkins for building and testing our code:

1. Create a new pipeline job and point it to your GitHub repo.
   
2. Add a Jenkinsfile to the repo with build, test, and deploy stages.

3. Configure a webhook in GitHub to trigger the pipeline on commits.

4. Run the pipeline and observe it checking out code, building the app, and archiving the output.

### Deploy to Elastic Beanstalk

Once our pipeline runs successfully, we can deploy manually to Elastic Beanstalk:

1. Download the source code ZIP file from GitHub.

2. Create a new Elastic Beanstalk environment.

3. Upload and deploy the ZIP file to the environment.

4. Access the running application.

### Troubleshooting:

Some problems encountered while creating the Jenkins pipeline:

- My build would get stuck at the Packaging of the output files stage and never complete.
  
- Checking the Jenkins logs showed the error:

`$ ERROR: Failed to bind to server socket: tcp://0.0.0.0:18050 due to: java.net.BindException: The socket name is already in use`

- This was caused by the Jenkinsfile having a input step to wait for manual input:

`$ input(message: 'Proceed to the next step?', ok: 'Continue')'

- The build would hang until clicking the "Continue" button in Jenkins to proceed past the input step.

- Removing the unnecessary input step allowed the packaging stage to complete automatically.



Our pipeline will do the following: 
1. Check out the code from GitHub.
2. Build the Python application.
3. Archive the source into a ZIP file.
4. Save the artifact to a location similar to `/var/lib/jenkins/jobs/myapp/builds/1/archive/myapp.zip`

## Optimization
Some ways this deployment could be improved:
- Use a Docker container for Jenkins for portability and consistency
- Add automated tests in the pipeline before packaging
  - Remove manual input steps: The pipeline had an input step to pause for manual input. This should be automated by removing the input or using a time-based wait instead.
- Leverage Infrastructure-as-Code tools like Terraform to provision AWS resources

Overall, this project successfully implemented a Jenkins CI/CD pipeline integrated with GitHub and Elastic Beanstalk for automated application delivery!
