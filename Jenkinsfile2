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
  - name: docker
    image: docker
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

        }
          container('docker') {
  
            sh 'docker --version'
            sh 'docker run --privileged -v /var/run/docker.sock:/var/run/docker.sock docker --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client \
  --env DOCKER_TLS_VERIFY=1'
            sh 'docker build -t cris/petclinic .'
            sh 'docker run -p 8080:8080 --user root -v /var/run/docker.sock:/var/run/docker.sock cris/petclinic'
    
         
        /*
                  docker {
            image 'docker:dind'
            args '-u root:root -p 3000:3000 --privileged -v /var/run/docker.sock:/var/run/docker.sock'
        }
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
