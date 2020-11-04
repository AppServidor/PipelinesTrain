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
          container('podman') {

          container('docker'){
            sh """
                docker login -u ${USER} -p ${PASS}'
                docker tag springclinic practicascristina/springclinic:latest
                docker push practicascristina/springclinic
            """
            
           
          }
            sh 'podman -v'
           // sh 'podman run --privileged -it -v tmp:/var/lib/containers:rw localhost/springclinic'
           // sh 'ls /var/run/containers/storage'
           // sh ' podman run -it --rm -v /var/run/containers/storage:/var/run/containers/storage -v /var/lib/containers/storage:/var/lib/containers/storage --storage-driver=overlay --privileged=true springclinic'
          //  sh 'podman run -p 8080:8080 --user root springclinic'
        
          /*   sh 'docker run --privileged -v /var/run/docker.sock:/var/run/docker.sock docker --env DOCKER_HOST=tcp://docker:2376 \
      --env DOCKER_CERT_PATH=/certs/client \
      --env DOCKER_TLS_VERIFY=1'
                sh 'docker build -t cris/petclinic .'
                sh 'docker run -p 8080:8080 --user root -v /var/run/docker.sock:/var/run/docker.sock cris/petclinic'
        */   
         
         
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
