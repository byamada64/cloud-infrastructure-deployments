# 🚀 AWS Elastic Beanstalk Deployment

## Build Summary

Provisioned an AWS Elastic Beanstalk environment to evaluate managed application deployment, platform abstraction, and automated scaling behavior in a production-aligned scenario.

This build focuses on PaaS tradeoffs versus infrastructure-based deployments, incorporating IAM roles, environment configuration, scaling policies, and application deployment workflows to validate operational simplicity, service exposure, and governed lab environment.


## 🏗️ Environment Build Choices

### Project Details

- **Platform:** AWS Elastic Beanstalk
- **Application Name:** nginx-demo-app
- **Environment Name:** nginx-demo-env
- **Environment Tier:** Web Server Environment
- **Region:** us-east-1
- **Domain:** Auto-generated Elastic Beanstalk URL

### Platform Configuration

- **Platform:** Docker
- **Platform Branch:** Docker running on 64bit Amazon Linux 2023
- **Platform Version:** 4.12.1

### Service Access

- **Service Role:** aws-elasticbeanstalk-service-role
- **EC2 Instance Profile:** aws-elasticbeanstalk-ec2-role
- **EC2 Key Pair:** Not configured for this lab

### Networking

- **VPC:** Default lab VPC
- **Public IP Address:** Enabled
- **Instance Subnet:** Single public subnet selected
- **Database:** Not enabled
- **Tags:** None assigned

### Instance and Scaling

- **Root Volume Type:** gp3
- **Root Volume Size:** 10 GB
- **Monitoring Interval:** 5 minutes
- **IMDSv1:** Disabled
- **EC2 Security Group:** Default VPC security group
- **Environment Type:** Single instance
- **Fleet Composition:** On-Demand
- **Processor Type:** x86_64
- **Instance Type:** t3.micro
- **Secondary Type Listed During Config:** t3.small
- **AMI:** Auto-selected by Elastic Beanstalk

### Monitoring and Updates

- **Health Reporting:** Enhanced
- **CloudWatch Custom Metrics:** Not configured
- **Log Streaming:** Disabled
- **Managed Platform Updates:** Enabled
- **Update Level:** Minor and patch
- **Deployment Policy:** All at once
- **Instance Replacement:** Disabled
- **Proxy Server:** nginx
- **X-Ray:** Disabled


## 💻 Commands Used

- `mkdir ~/beanstalk-v1` — created first local application folder
- `cat > index.html <<EOF ... EOF` — created first static HTML file
- `zip -r ../beanstalk-v1.zip .` — packaged first upload artifact
- `mkdir ~/beanstalk-docker-v1` — created corrected Docker-based application folder
- `cat > index.html <<EOF ... EOF` — created custom Beanstalk landing page
- `cat > Dockerfile <<EOF ... EOF` — created Dockerfile for nginx container deployment
- `zip -r ../beanstalk-docker-v1.zip .` — packaged Docker deployment artifact
- `ls -lh ~/beanstalk-v1.zip` — validated first ZIP output
- `ls -lh ~/beanstalk-docker-v1.zip` — validated corrected Docker ZIP output


## ✅ Validation

- Elastic Beanstalk environment launched successfully
- Environment health reached **Ok**
- Public Elastic Beanstalk domain was generated
- Docker platform on Amazon Linux 2023 deployed successfully
- IAM service role created successfully
- IAM EC2 instance profile created successfully
- Environment events confirmed EC2 instance creation
- Deployment history showed successful environment creation
- Health dashboard displayed instance health as **Ok**
- Live public page loaded successfully
- Custom Docker-based Brian landing page displayed successfully


## 🧠 Lessons Learned

- Elastic Beanstalk automates much of the infrastructure required to host an application, but the deployment artifact still must match the chosen platform
- A Docker-based Beanstalk environment requires a valid `Dockerfile` or `Dockerrun.aws.json`
- A static HTML file alone is not sufficient when the environment expects a Docker workload
- Failed deployments can still leave the environment healthy because Elastic Beanstalk rolls back to the last known good version
- Event logs and CloudFormation status messages are critical for troubleshooting deployment failures
- IAM role separation matters: the service role and EC2 instance profile serve different functions
- Enhanced health reporting provides useful operational visibility even in simple labs
- Managed services reduce manual work, but engineers still need to understand what is happening underneath
- Cleanup discipline matters just as much as successful deployment


## 🔮 Future Improvements

- Deploy a second custom version to practice repeatable release management
- Connect the environment to GitHub for CI/CD-based deployments
- Enable CloudWatch log streaming for deeper observability
- Add environment variables and application configuration settings
- Test HTTPS / SSL behavior and custom domain mapping
- Explore multi-instance and load-balanced environments
- Compare Elastic Beanstalk directly to AWS EC2 manual hosting and Azure App Service
- Deploy a real application instead of a lightweight landing page


## 🧹 Final Hygiene Cleanup

- Terminated Elastic Beanstalk environment after validation
- Confirmed CloudFormation stack entered `DELETE_IN_PROGRESS`
- Released public environment URL
- Removed associated compute resources through environment termination
- Maintained clean sandbox hygiene to avoid unnecessary AWS spend
