
pipeline {
  options {
    buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
  }
  agent any

  tools {
    maven 'maven_3.9.4'
  }

  stages {
    stage('Code Compilation') {
      steps {
        echo 'Code Compilation Is In Progress!'
        sh 'mvn clean compile'
        echo 'Code Compilation is Completed Successfully!'
      }
    }
    stage('Code QA Execution') {
      steps {
        echo 'Junit Test Case check in Progress!'
        sh 'mvn clean test'
        echo 'Junit Test Case check Completed!'
      }
    }
    stage('Code Package') {
      steps {
        echo 'Creating War Artifact'
        sh 'mvn clean package'
        echo 'Creating War Artifact done'
      }
    }
    stage("Building & Tag Docker Image") {
        steps {
                echo "Starting Building Docker Image"
                sh "docker build -t abhiyadav2294/flipkart-ms:${BUILD_NUMBER},"
                sh "docker build -t flipkart-ms:dev-flipkart-v1.${BUILD_NUMBER} ."
                echo 'Completed Building Docker Image'
             }

    }
    stage("Docker Image Scanning") {

    steps {
             echo 'Docker Image Scanning Started!
             sh 'java -version'
             echo 'Docker Image Scanning Started'
           }
    }

     stage('Docker push to Docker Hub') {

     steps {
          script {
                   withCredentials([string(credentialsId: 'dockerhubCred', variable: 'dockerhubCred')]) {
                    sh "docker Login docker 10 -u abhiyadav2294 -p $(dockerhubCred}"
                   echo "Push Docker Image to DockerHub: In Progress"
                    sh "docker push abhiyadav2294/flipkart-ms:dev-flipkart-v1.  ${BUILD_NUMBER}"
                    echo "Push Docker Image to DockerHub: Completed"
                  }
                }
              }
       }

       stage("Docker Image Push to Amazon ECR") {
            steps {
                  script {
                           withDockerRegistry([credentialsId: 'ecr:us-east-2: ecr-credentials', url: "https://782531903514.dkr.ecr.us-east-2.amazonaws.com"]){
                            sh """
                           echo "Tagging the Docker Image: In Progress"
                           docker tag flipkart-ms: dev-flipkart-v1.${BUILD_NUMBER) 782531903514.dkr.ecr.us-east-2.amazonaws.com/flipkart-ms:$(BUILD_NUMBER}
                           echo "Tagging the Docker Image: Completed"
                           echo "Push Docker Image to ECR: In Progress"
                           docker push 782531903514.dkr.ecr.us-east-2.amazonaws.com/flipkart-ms:${BUILD_NUMBER}
                           echo "Push Docker Image to ECR: Completed"
                           """
                        }
                      }
                    }
                  }
                }
             }