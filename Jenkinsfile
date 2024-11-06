pipeline {
   agent any
  
   environment {
       DOCKER_HUB_REPO = "chanduvasa/flask-hello-world"
       CONTAINER_NAME = "flask-hello-world"
   }
  
   stages {
    //    stage('Checkout') {
    //        steps {
    //            checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[
    //                url: 'https://github.com/ChanduVasa/jenkins.git',
    //            ]]])
    //        }
    //    }
       stage('Build') {
           steps {
               echo 'Building..'
               bat 'docker image build -t chanduvasa/flask-hello-world:latest .'
           }
       }
       stage('Test') {
           steps {
               echo 'Testing..'
               bat 'docker stop %CONTAINER_NAME% || exit 0'  // stop if running, ignore error if not
               bat 'docker rm %CONTAINER_NAME% || exit 0'  // remove if exists, ignore error if not
               bat 'docker run --name %CONTAINER_NAME% %DOCKER_HUB_REPO% /bin/bash -c "pytest test.py && flake8"'
           }
       }
       stage('Deploy') {
           steps {
               echo 'Deploying....'
               bat 'docker stop %CONTAINER_NAME% || exit 0'
               bat 'docker rm %CONTAINER_NAME% || exit 0'
               bat 'docker run -d -p 8080:8080 --name %CONTAINER_NAME% %DOCKER_HUB_REPO%'
           }
       }
   }
}