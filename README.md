# DevSecOps pipeline for Python project

A Jenkins end-to-end DevSecOps pipeline for Python web application.

<img src="https://github.com/adavarski/DevSecOps-pipeline-python/blob/main/pictures/DevSecOps-pipeline-full.png" width="900">

Jenkins instance/environment hosted on AWS EC2 (Ubuntu 18.04) or local environment (laptop/on-prem ubuntu server).

Features:

- [x] Select appropriate security tools and sample python project
- [x] Set up Jenkins server using docker (Dockerfile) and pipeline as code (Jenkinsfile) to run the checks
- [x] Use ansible to create AWS ec2 test instance, configure the environment, and interact with it
- [x] Hook up the web-app with modsecurity providing WAF, reverse proxy capabilities
- [x] Bootstrap with Jenkins API/configfile to setup and automatically create the pipeline job
- [x] Carry out authenticated DAST scan on the python web app 



*Disclaimer: This project is for demonstration purpose with surface level checks only, do not use it as-is for production*

> **Checkout project** - check out python application project repository with XSS vulnerability

> **git secret check** - check there is no password/token/keys/secrets accidently commited to project github (trufflehog)

> **SCA** - check external dependencies/libraries used by the project have no known vulnerabilities (safety)

> **SAST** - static analysis of the application source code for exploits, bugs, vulnerabilites (Bandit)

> **Container audit** - audit the container that is used to deploy the python application (Lynis)

> **DAST** - deploy the application, register, login, attack & analyse it from the frontend as authenticated user (Nikto + Selenium + python custom script)

> **System security audit** - analyse at the security posture of the system hosting the application (Lynis)

> **WAF** - deploy application with WAF which will filter malicious requests according to OWASP core ruleset (owasp/modsecurity-crs)


## Installation steps

1. Clone this repository to your Ubuntu Server (AWS EC2 t2-medium recommended) or to your laptop/local workstation.
```
git clone https://github.com/adavarski/DevSecOps-pipeline-python
```

2. Edit the code to make it work on your AWS
   - Change to your AWS subnet [vpc_subnet_id](jenkins_home/createAwsEc2.yml#L30) 
   - Change to your AWS [security_group](jenkins_home/createAwsEc2.yml#L10) (allow inbound ssh(22), WAF(80), *Optional* web-app(10007) from your IP ONLY)
   - AWS IAM: Create account, give full-ec2-access to this account. Create AWS Access keys for this account and get access key ID and secret access key.
   - (optional: if we use AWS EC2 to host jenkins): Create an IAM role which gives full-ec2-access and assign it to your ubuntu server (AWS EC2 jenkins instance)

3. Run the setup script to create CI/CD server with Jenkins+pipeline ready to go

Uncomment all needed lines @setup-ubuntu.sh

If using jenkins local (your laptop) environment
```
cd DevSecOps-pipeline-python
./setup-ubuntu.sh
```

If using AWS EC2 jenkins environment.
```
cd DevSecOps-pipeline-python
sudo sh setup-ubuntu.sh 
```

4. Make sure your firewall allows incoming traffic to port 8080 (if using AWS EC2 to host jenkins). Then, go to your jenkins server URL 

```
http://your-jenkins-server:8080/
```

For local jenkins environment:
```
https://localhost:8080
```

5. Use the temporary credentials provided on the logs to login. Change your password!
6. Go to the python pipeline project dashboard, click on "Build Now" button to start it off -> "Build with parameters"-> Enter AWS credentials (aws_access_key_id & aws_secret_access_key)


## Setting up a Jenkins Pipeline project manually
 
**A sample pipeline is already provided through automation**

1. Click on New Item, input name for your project and select Pipeline as the option and click OK.
2. Scroll down to Pipeline section - Definition, select "Pipeline script from SCM" from drop down menu.
3. Select Git under SCM, and input Repository URL.
4. (Optional) Create and Add your credentials for the Git repo if your repo is private, and click Save.
5. You will be brought to the Dashboard of your Pipeline project, click on "Build Now" button to start off the pipeline-> -> "Build with parameters"-> Enter AWS credentials (aws_access_key_id & aws_secret_access_key)


**To do list:**


## Demo

<img src="https://github.com/adavarski/DevSecOps-pipeline-python/blob/main/pictures/DevSecOps-pipeline-steps-UI.png" width="900">

<img src="https://github.com/adavarski/DevSecOps-pipeline-python/blob/main/pictures/DevSecOps-ec2.png" width="900">

### Reports

<img src="https://github.com/adavarski/DevSecOps-pipeline-python/blob/main/pictures/DevSecOps-workspace.png" width="500">


