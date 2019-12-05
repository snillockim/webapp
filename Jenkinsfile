pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage ('Check-Git-Secrets'){
            steps {
                sh 'rm trufflehog || true'
                sh 'docker run gesellix/trufflehog --json https://github.com/snillockim/webapp.git > trufflehog'
                sh 'cat trufflehog'
            }
        }
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                    ''' 
            }
        }
        stage ('Build') {
        steps {
        sh 'mvn clean package'
        }
    }
         stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@34.251.116.128:/home/ubuntu/prod/apache-tomcat-9.0.29/webapps/webapp.war'
              }      
           }       
    }
    }
}
