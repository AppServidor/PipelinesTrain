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
        stage('Example Build') {
         steps {
        container('maven') {
          sh 'mvn -version'
          sh 'mvn clean install'
        }
      }
        }
        stage('Example Test') {
           
            steps {
                echo 'Hello, JDK'
                sh 'java -version'
               
            }
      
        }
}
 post {
        always {
            archiveArtifacts artifacts: 'build/libs/**/*.jar', fingerprint: true
           
        }
    }
}
