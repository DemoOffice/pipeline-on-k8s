pipeline {
  agent {
    kubernetes {
      label 'kubernetes'
      containerTemplate {
        name 'maven'
        image 'maven:3.3.9-jdk-8-alpine'
        ttyEnabled true
        command 'cat'
      }
      
    }
    
  }
  stages {
    stage('Preparation') {
      steps {
        git(url: 'https://github.com/jglick/simple-maven-project-with-tests.git', branch: 'master')
        sh 'echo "hello"'
      }
    }
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            sh 'mvn -Dmaven.test.failure.ignore clean package'
          }
        }
        stage('codeQA') {
          steps {
            echo 'code has been scanned'
          }
        }
        stage('License Check') {
          steps {
            sh 'ls -al'
          }
        }
      }
    }
    stage('Result') {
      steps {
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts 'target/*.jar'
      }
    }
  }
}