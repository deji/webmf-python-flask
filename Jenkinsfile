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
        sh 'echo "opportunity to change things and here we go again push"'
        sh 'pip install -r requirements.txt'
        sh '''
          cat > build.properties <<EOF
          TYPE=docker/image
          REFERENCE=oladejif/t8-repo-jenkins-spinnaker:second-4
          NAME=oladejif/t8-repo-jenkins-spinnaker
          EOF
        '''.stripIndent()
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
  post {
      always {
          archiveArtifacts artifacts: 'build.properties', onlyIfSuccessful: true
      }
  }
}
