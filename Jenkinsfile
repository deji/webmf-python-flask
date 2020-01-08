pipeline {
  agent {
    kubernetes {
      //cloud 'kubernetes'
      defaultContainer 'python'
      yaml """
kind: Pod
spec:
  containers:
  - name: python
    image: python:3.7.2
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage('build') {
      steps {
        sh 'pip install -r requirements.txt'
      }
    }
    stage('test') {
      steps {
        sh 'python test.py'
      }
      post {
        always {
          junit 'test-reports/*.xml'
        }
      }    
    }
  }
}
