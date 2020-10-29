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
  - name: petclinic
    image: paulczar/petclinic:spring-k8s-1
    command:
    - cat
    tty: true
 
"""
    }
  }
  stages {
    stage('Run') {
      steps {
        container('petclinic') {
          sh 'echo "hola"'
        }
      }
    }
  }
}
