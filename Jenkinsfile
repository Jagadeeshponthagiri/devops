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
                SONAR_URL = "http://34.201.51.40:9000"
                PATH = "/opt/apache-maven-3.9.5/bin:$PATH"
            }
            steps{
             withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
              sh 'mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
                } 
            }
          }
        stage('docker build and push'){
            environment{
              DOCKER_REGISTRY_CREDENTIALS = credentials('docker-cred')
              DOCKER_IMAGE_NAME = 'jagadeeshponthagiri/ultimate-cicd:${BUILD_NUMBER}'    
            }
            steps{
              script{
                sh 'cd Dockerjenk/springboot/Dockerfile && docker build -t ${DOCKER_IMAGE} .'
                docker.withRegistry('https://index.docker.io/v1/', "DOCKER-cred") {
                  docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}").push()
                }
              }
            }
        }
     }        
    }

