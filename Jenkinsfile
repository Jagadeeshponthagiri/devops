pipeline {
    agent any

    stages {
        stage('git') {
            steps {
                git url :  'https://github.com/Jagadeeshponthagiri/devops'
            }
        }
        stage('build') {
            environment{
                PATH = "/opt/apache-maven-3.9.5/bin:$PATH"
            }
            steps {
                sh "mvn clean package"
            }
        }
        stage('static code analysis') {
            environment{
                SONAR_URL = "http://44.207.3.111:9000"
                PATH = "/opt/apache-maven-3.9.5/bin:$PATH"
            }
            steps{
             withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
              sh 'mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
                } 
            }
          }
    }
}
