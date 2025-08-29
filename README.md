## Deploying Java Application using Apache Tomcat using Maven Project Pipeline

### Requirements
  - EC2 instance(AMZ linux)
  - java 17 install
  - apache tomcat install & setup user in conf

  ``
        <role rolename="manager-gui"/>
        <user username="tomcat" password="tomcat" roles="manager-gui"/>
        <role rolename="admin-gui"/>
        <user username="admin" password="admin123" roles="manager-gui,admin-gui"/>
  ``

  - jenkins install
  - plugins - Maven
            - maven Integration Plugin
            - deploy to container
  - Credentials of apache tomcat set in jenkins
      username - admin
      password - admin 
    
## Steps
1. Store apache tomcat credentials in jenkins
   manage jenkins > credentials
2. Install Plugins
   Manage jenkins > plugins - 
            - maven Integration Plugin
            - deploy to container
3. create a pipline using Maven Project
            - git - url
            - branch - */main
            - build - pom.xml
            - goals and options - clean install package
            - post build actions - Deploy war/ear to a container
            - WAR/EAR files - "**/*.war"
            - containers - tomcat 9.x remote
            - credentials - tomcat-user(you store in jenkins)
            - tomcat url - public_ip:8080


## Using Descriptive pipeline 
pipeline {  

    agent any
        
    tools{
        maven "maven"
    }
    stages {
        stage('Clone') {
            steps {
               git 'https://github.com/hitu-1995/maven-web-app.git'
            }
        }
        stage('Build') {
            steps {
               sh 'mvn clean install'
            }
        }
        stage('Deploy to Tomcat'){
            steps{
                deploy adapters:[tomcat9(credentialsId:'tomcat-user',path:'',url:"tomcat_url")],contextPath:null,war:'**/*.war'
            }
        }
    }
}

