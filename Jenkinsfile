pipeline {
  agent {
    label 'master'
    kubernetes {
      yaml """
apiVersion: apps/v1
kind: Deployment
metadata:
  name: petclinic-deployment
  labels:
    app:  petclinic_server
spec:
  replicas: 1
  selector:
    matchLabels:
      app: petclinic
  template:
    metadata:
      labels:
        app: petclinic
    spec:
      containers:
      - name: petclinic
        image: paulczar/petclinic:spring-k8s-1
        imagePullPolicy: Always
        ports:
        - containerPort: 8080
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
