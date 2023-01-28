pipeline {
  agent any
  tools {
    maven 'maven'
  }
  environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
  }
  
  stages {
    stage('build') {
      steps {
         sh 'mvn clean package'
      }
    }
    stage('docker build') {
      steps {
        sh "docker build -t shaggalok24/hello-world:${TAG} -f /home/ec2-user/Dockerfile"
        
      }
      }
    
    stage('push image') {
      steps {
        script {
            docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            docker.image("shaggalok24/hello-world:${TAG}").push()
            docker.image("shaggalok24/hello-world:${TAG}").push("latest")
         }
       }
      }
    }
    stage('Deploy'){
            steps {
                sh "docker stop hello-world | true"
                sh "docker rm hello-world | true"
                sh "docker run --name hello-world -d -p 9004:8080 shaggalok24/hello-world:${TAG}"
            }
        }
    }
}
