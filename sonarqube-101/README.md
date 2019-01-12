# How-to deploy to Azure SonarQube

## Introduction
SonarQube is an open source platform to perform automatic reviews with static analysis of code to detect bugs, code smells and security vulnerabilities on 25+ programming languages including Java, C#, JavaScript, TypeScript, C/C++, COBOL and more. SonarQube is the only product on the market that supports a leak approach as a practice to code quality.

SonarQube is an open-source tool that assists in code quality analysis and reporting. It scans your source code looking for potential bugs, vulnerabilities, and maintainability issues, and then presents the results in a report which will allow you to identify potential issues in your application.

In this tutorial, we will learn how to deploy ready-to-use SonarQube environment on Azure.

## Architecture
To reproduce the solution we will need to download and run [script.sh](https://raw.githubusercontent.com/groovy-sky/azure/master/sonarqube-101/script.sh), which will:
1. Create a Linux Virtual Machine and a PostgreSQL instance in Azure using [azuredeploy.json](https://raw.githubusercontent.com/groovy-sky/azure/master/sonarqube-101/azuredeploy.json) file.
1. Install NGINX and Certbot for providing secure access to SonarQube
1. Install Docker and Docker compose for running containerized SonarQube instance using [docker-compose.yml](https://raw.githubusercontent.com/groovy-sky/azure/master/sonarqube-101/docker-compose.yml) template

As a result we will get running VM (with NGINX and dockerized SonarQube) and PostgreSQL instance (used as a database for SonarQube):
![](/images/sonarqube-101/sonar_arch.png)

## Prerequisites
To complete this tutorial, we will need:
* Active [Azure subscription](https://azure.microsoft.com/en-us/pricing/purchase-options/)
* Some Linux environment (Ubuntu, Debian, Centos, Suse or Windows Subsystem for Linux) with installed 'jq' package on it
* [Azure CLI 2 version](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

## Implementation
There is one thing which should be done before running a deployment - we need to create a new resource group:
![](/images/sonarqube-101/azure_new_group.png)

Now we can open our Linux environment (in this example has beend used Ubuntu on Linux), download [script.sh](https://github.com/groovy-sky/azure/raw/master/sonarqube-101/script.sh) file and execute it. To run it we will need to use following 3 paramaters (check the image below): 
* Azure subscription Id
* Azure deployment resource group name
* Password for PostgreSQL

```
PostgreSQL password must be at least 8 characters in length and contain characters from three of the following categories – English uppercase letters, English lowercase letters, numbers (0-9), and non-alphanumeric characters (!, $, #, %, etc.).
```

![](/images/sonarqube-101/deploy_param.png)

Also, during the script execution, we will be asked for a password for the VM. Please provide password which conform to [password requirements](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm):

![](/images/sonarqube-101/vm_password.png)

The deployment could take about 25-40 minutes. After it will be finished, we can open newly created virtual machine, copy it DNS Name and access SonarQube thru HTTPS:
![](/images/sonarqube-101/result.png)

SonarQube ships with a default administrator username and password - [admin/admin](https://docs.sonarqube.org/latest/instance-administration/security/#header-2):
![](/images/sonarqube-101/admin_login.png)

Such password is not secure, so we'll want to update it:
![](/images/sonarqube-101/sonar_admin_pass.png)

Another important security breach is that the SonarQube instance is wide-open to the world, and anyone could view analysis results and of a source code. This setting is highly insecure, so we'll configure SonarQube to only allow logged-in users access to the dashboard:
![](/images/sonarqube-101/sonar_off_anonym.png)

## Conclusion
In this tutorial, we've set up a SonarQube instance and secure it. Now you're ready to [install an analyzer and begin creating projects](https://docs.sonarqube.org/latest/analysis/overview/). 

## Useful documentation

[Get started with Docker Compose
](https://docs.docker.com/compose/gettingstarted/)

[SonarQube docker image](https://hub.docker.com/_/sonarqube)

[About SonarQube security](https://docs.sonarqube.org/latest/instance-administration/security/)

[Another article about installing and configuring SonarQube](https://www.digitalocean.com/community/tutorials/how-to-ensure-code-quality-with-sonarqube-on-ubuntu-16-04)