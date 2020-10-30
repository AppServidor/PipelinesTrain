pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: maven
    image: maven:alpine
    command:
    - cat
    tty: true
  - name: openjdk
    image: openjdk
    command:
    - cat
    tty: true

"""
    }
  }
  stages {
    stage('Run maven') {
      steps {
        container('maven') {
          sh 'mvn -version'
          sh 'mvn clean install'
        }
      }
    
        stage ('Build') {

            steps {
              sh 'make' 
              archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true 
            }

        }
        
    }
  }
}
