
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

                 }
                }

