pipeline {

  agent any
  tools { 
        maven 'Maven'
        jdk 'Java'
  }
  stages {
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace... */
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
        sh 'echo $USER'
        sh 'echo whoami'
      }
    }
    stage('Docker Build') {
      steps {
        sh 'docker build -t employee-service .'
      }
    }   
    stage('push image to DockerHub'){
      steps {
         withDockerRegistry(credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/') {
         sh 'docker tag vamshi533/vamshik8s:employee-service'
         sh 'docker push vamshi533/vamshik8s:employee-service'  
         }
      }
    }
    stage('deploy to kubernetes') {
      steps {
          checkout scm
         sh 'kubectl apply -f deployment.yaml' 
         sh 'kubectl apply -f service.yaml' 
      }
    }
  }
 }
