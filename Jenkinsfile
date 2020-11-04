pipeline {
   environment {
        USER = 'practicascristina'
        PASS = 'Clsrz94;;'
    }
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
  - name: podman
    image: marshallford/podman
    command:
    - cat
    tty: true
  - name: docker
    image: docker
    command:
    - cat
    tty: true
  - name: openjdk
    image: openjdk
    command:
    - cat
    tty: true
  - name: buildah
    image:  buildah/buildah
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
            sh 'mvn clean package'
            stash includes: 'target/*.jar', name: 'targetfiles'

          }
          container ('buildah'){
            sh 'buildah bud -t springclinic .'
            sh 'buildah images'
            sh 'buildah containers'
          }
      container('docker'){
            sh """
                docker login -u ${USER} -p ${PASS}'
                docker tag springclinic practicascristina/springclinic:latest
                docker push practicascristina/springclinic
            """
            
           
          }

      }
    }
   /*     stage('Example Test') {
           
            steps {
                echo 'Hello, JDK'
                sh 'java -version'
               
            }
      
        }*/ 
}

}
