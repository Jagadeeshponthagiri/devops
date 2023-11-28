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
                SONAR_URL = "http://44.204.227.198:9000"
                PATH = "/opt/apache-maven-3.9.5/bin:$PATH"
            }
            steps{
             withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
              sh 'mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
                } 
            }
          }
        stage('docker build'){
            environment{
              DOCKER_HUB_CREDENTIALS = 'docker-cred'
              DOCKER_IMAGE_NAME = 'demo-job'
              DOCKER_HUB_REPO = 'jagadeeshponthagiri/index.docker.io'    
            }
            steps{
              script{
                sh "docker build -t ${DOCKER_IMAGE_NAME}"
                }
              }
        stage('docker push'){
            steps{
              script{
                withDockerRegistry(credentialsId: "${DOCKER_HUB_CREDENTIALS}", url: '') {
                  sh "docker tag ${DOCKER_IMAGE_NAME} ${DOCKER_HUB_REPO}:${BUILD_NUMBER}"
                  sh "docker push ${DOCKER_HUB_REPO}:${BUILD_NUMBER}"
                }
              }
          }
       }
    }
}
