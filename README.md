# Cloud Native - Final Project 01

## Summary:
    
   - [Introduction](https://github.com/gabikersul/poc-rest-service#introduction)
   - [Configuration](https://github.com/gabikersul/poc-rest-service#configuration)
   - [Job #1 - Build App](https://github.com/gabikersul/poc-rest-service#job-1---build-app)
   - [Job #2 - Infra](https://github.com/gabikersul/poc-rest-service#job-2---infra)
   - [Job #3 - Run](https://github.com/gabikersul/poc-rest-service#job-3---run)
   - [Deploy](https://github.com/gabikersul/poc-rest-service#deploy)
## Introduction
It's a 'Greeting' Java + Springboot Application with three Jenkins jobs:
- Job #1 - Build: it downloads the project code from a github repo, run the tests,
 build the project, generate the war file and uploads it to a JFrog Artifactory repo;
- Job #2 - Infra: it downloads the war file from the JFrog Artifactory repo,
runs the packer build command which provision a docker image, copy the downloaded
war file to the image and uses ansible to provision an openjdk8 installation;
- Job #3 -Run: it pulls the published image from docker hub and run it inside a container;

- Youtube video where I show and comment what is happening during the pipelines execution:
  - youtube_link_here

## Configuration
- The following ports must be available: **8080**, **8081**, **8082** and **8083**;

- Download [Jenkins](https://www.jenkins.io/download/);
- Download [Packer](https://www.packer.io/downloads.html);
- Download [Docker](https://docs.docker.com/get-docker/): 
    - To avoid typing sudo while using Docker execute this command in your terminal:
    
            sudo usermod -aG docker ${USER}
    
    - To apply the changes restart your machine.

        
- Execute the _artifactory.sh_ script to download JFrog docker image and run it:

        sudo bash artifactory.sh

- Jenkins:   
    - Access Jenkins at http://localhost:8080 (default);
    - Create your user and password and install the recommended plugins;
    
- JFrog Artifactory:
    - Access Jfrog Artifactory at http://localhost:8081/artifactory (default);
    - Login with the default credentials: 
        - user: admin password: password
    - Set your new admin password;
    - Go to Welcome, admin > Quick Setup and create a gradle repository.

- Artifactory Plugin:
    - Go to Manage Jenkins > Manage Plugins > Available. 
    - Search for 'Artifactory' plugin and install it;
    - Go to Manage Jenkins > Configure System and scroll down until you find Artifactory:
    - Choose a Jfrog Artifactory server ID name;
    - Insert the Jfrog Artifactory URL (default: http://IP_ADDRESS:8081/artifactory);
    - Type your JFrog Artifactory login.
    - Apply and save.
    

- Artifactory credentials:
    - Go to Jenkins > Credentials > System > Global Credentials (unrestricted);
    - Click on Add Credentials, select "Username with a password";
    - Type your JFROG Artifactory login;
    - In the "ID" field insert 'artifactory_credentials' and save.
    - Click on Add Credentials, select "Secret text";
    - In "Secret" insert your JFrog artifactory server ID, in "ID" insert "artifactory_server_id" and save;
    - Click on Add Credentials, select "Secret text";
    - In "Secret" insert your artifactory url, insert "artifactory_server_url" in "ID" and save;

        
- Docker Hub credentials:
    - Before: Create an account in [Docker Hub](https://hub.docker.com/signup) (skip this step if you already have one);
    - Go to Jenkins > Credentials > System > Global Credentials (unrestricted);    
    - Click on Add Credentials, select "Username with a password";
    - Type your Docker Hub login;
    - Insert 'docker_hub_credentials' in the ID field and save.

## Job #1 - Build app 

- Access Jenkins;
- Go to New Item, in "Enter an item name" insert "build-app";
- Select "Pipeline" and click OK;
- Scroll down to "Pipeline", in "Definition" select "Pipeline script from SCM" ;
- In "SCM" select "Git" ;
- In "Repository URL" insert https://github.com/gabikersul/poc-rest-service.git ;
- In "Script Path" insert "build-app/Jenkinsfile" ;
- Apply and save.
        
## Job #2 - Infra 

- Access Jenkins;
- Go to New Item, in "Enter an item name" insert "infra";
- Select "Pipeline" and click OK;
- Scroll down to "Pipeline", in "Definition" select "Pipeline script from SCM" ;
- In "SCM" select "Git" ;
- In "Repository URL" insert https://github.com/gabikersul/poc-rest-service.git ;
- In "Script Path" insert "infra/Jenkinsfile" ;
- Apply and save.

## Job #3 - Run 

- Access Jenkins;
- Go to New Item, in "Enter an item name" insert "run";
- Select "Pipeline" and click OK;
- Scroll down to "Pipeline", in "Definition" select "Pipeline script from SCM" ;
- In "SCM" select "Git" ;
- In "Repository URL" insert https://github.com/gabikersul/poc-rest-service.git ;
- In "Script Path" insert "run/Jenkinsfile" ;
- Apply and save.

## Deploy
 #### To deploy the application:
 1. Access build-app job and click on "Build now";
 2. Access infra job and click on "Build now";
 3. Access run job and click on "Build now"; 
 - You can access the application at http://localhost:8083/greeting which shows the default message 
 - And http://localhost:8083/greeting?name=your_name_here (change 'your_name_here' for a name of your choice) 
    which shows a tailored message.