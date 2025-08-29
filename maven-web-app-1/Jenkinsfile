pipeline {  

    agent any
        
    tools{
        maven "maven"
    }
    stages {
        stage('Checkout') {
            steps {
               checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kaustubh2949/maven-web-app.git']])
            }
        }
        stage('Build') {
            steps {
               sh 'mvn clean install'
            }
        }
        stage('Deploy to Tomcat'){
            steps{
                deploy adapters:[tomcat9(credentialsId:'tomcat-user',path:'',url:"http://192.168.200.128:8081/")],contextPath:null,war:'**/*.war'
            }
        }
    }
}


