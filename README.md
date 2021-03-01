# DevSecOps pipeline for Python project

A Jenkins end-to-end DevSecOps pipeline for Python web application, hosted on AWS Ubuntu 18.04

<img src="https://github.com/adavarski/DevSecOps-pipeline-python/blob/main/pictures/DevSecOps-pipeline-full.png" width="900">


*Disclaimer: This project is for demonstration purpose with surface level checks only, do not use it as-is for production*

> **Checkout project** - check out python application project repository with XSS vulnerability

> **git secret check** - check there is no password/token/keys/secrets accidently commited to project github

> **SCA** - check external dependencies/libraries used by the project have no known vulnerabilities

> **SAST** - static analysis of the application source code for exploits, bugs, vulnerabilites

> **Container audit** - audit the container that is used to deploy the python application

> **DAST** - deploy the application, register, login, attack & analyse it from the frontend as authenticated user

> **System security audit** - analyse at the security posture of the system hosting the application

> **WAF** - deploy application with WAF which will filter malicious requests according to OWASP core ruleset


## Installation steps

1. Clone this repository to your Ubuntu Server (t2-medium recommended)
```
git clone https://github.com/adavarski/DevSecOps-pipeline-python
```

2. Edit the code to make it work on your AWS
   - Change to your AWS subnet [vpc_subnet_id](jenkins_home/createAwsEc2.yml#L30) 
   - Change to your AWS [security_group](jenkins_home/createAwsEc2.yml#L10) (allow inbound ssh(22), WAF(80), *Optional* web-app(10007) from your IP ONLY)
   - Create an IAM role which gives full-ec2-access and assign it to your ubuntu server

3. Run the setup script to create CICD server with Jenkins+pipeline ready to go
```
cd DevSecOps-pipeline-python
./setup-ubuntu.sh

# (OR: sudo sh setup-ubuntu.sh)
```

4. Make sure your firewall allows incoming traffic to port 8080. Then, go to your jenkins server URL 
```
http://your-jenkins-server:8080/
```

Note: If local install 
```
https://localhost:8080
```
5. Use the temporary credentials provided on the logs to login. Change your password!
6. Go to the python pipeline project dashboard, click on "Build Now" button to start it off.(befor that configure AWS credentials into Jenkinsfile: this GitHub repo, make repo private)  

## Setting up a Jenkins Pipeline project manually

1.Setup J.pipeline using Jenkinsfile.example1 or Jenkinsfile.example2, edit AWS credentials
 
**A sample pipeline is already provided through automation**

1. Click on New Item, input name for your project and select Pipeline as the option and click OK.
2. Scroll down to Pipeline section - Definition, select "Pipeline script from SCM" from drop down menu.
3. Select Git under SCM, and input Repository URL.
4. (Optional) Create and Add your credentials for the Git repo if your repo is private, and click Save.
5. You will be brought to the Dashboard of your Pipeline project, click on "Build Now" button to start off the pipeline.


**To do list:**
- [x] Select appropriate security tools and sample python project
- [x] Set up Jenkins server using docker (Dockerfile) and pipeline as code (Jenkinsfile) to run the checks
- [x] Use ansible to create AWS ec2 test instance, configure the environment, and interact with it
- [x] Hook up the web-app with ~~nginx~~+modsecurity providing WAF, ~~DDoS protection~~, reverse proxy capabilities
- [x] Bootstrap with Jenkins API/configfile to setup and automatically create the pipeline job
- [x] Carry out authenticated DAST scan on the python web app 

## Demo

<img src="https://github.com/adavarski/DevSecOps-pipeline-python/blob/main/pictures/DevSecOps-pipeline-steps-UI.png" width="900">

<img src="https://github.com/adavarski/DevSecOps-pipeline-python/blob/main/pictures/DevSecOps-ec2.png" width="900">

### Report

<img src="https://github.com/adavarski/DevSecOps-pipeline-python/blob/main/pictures/DevSecOps-workspace.png" width="900">


