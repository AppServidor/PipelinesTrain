pipeline {
   environment {
        USER = 'practicascristina'
        PASS = 'Crlsrz9489'
        imageName = 'springclinic'
        imageTag = 'latest'
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
  - name: docker
    image: docker:dind
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
  - name: helm
    image:  alpine/helm
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
            sh 'buildah bud --format=docker -t ${imageName} .'
            sh 'buildah images'
            sh 'buildah login -u ${USER} -p ${PASS} docker.io'
            sh 'buildah tag ${imageName} practicascristina/springrepo'
            sh 'buildah push --creds=${USER}:${PASS} practicascristina/springrepo'
     //--creds=${USER}:${PASS}
            //sh 'buildah  push docker.io/practicascristina/${imageName}:${imageTag}'   
           
          }
          container ('helm'){
            sh 'helm repo add bitnami https://charts.bitnami.com/bitnami'
            sh 'helm install my-petclinic-app --set image.repository=practicascristina/springrepo --set image.tag=latest --set image.pullPolicy=Always bitnami/tomcat'
            sh 'helm repo update' 
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
