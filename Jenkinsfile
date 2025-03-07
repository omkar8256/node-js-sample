  pipeline {
       agent any

       environment {
           DOCKER_CREDENTIALS_ID = 'dockerhub-credentials'
           DOCKER_IMAGE = 'omkar8256/node-js-sample'
           HELM_CHART_DIR = './helm-chart/node-js-sample'
       }

       stages {
           stage('Clone') {
               steps {
                   git 'https://github.com/omkar8256/node-js-sample.git'
               }
           }
           stage('Build') {
               steps {
                   script {
                       sh 'npm install'
                   }
               }
           }
           stage('Docker Build') {
               steps {
                   script {
                       sh "docker build -t ${DOCKER_IMAGE} ."
                   }
               }
           }
           stage('Push to Docker Hub') {
               steps {
                   script {
                       withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                           sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin"
                           sh "docker push ${DOCKER_IMAGE}:latest"
                       }
                   }
               }
           }
           stage('Helm Deploy') {
               steps {
                   script {
                       sh "helm upgrade --install node-js-sample ${HELM_CHART_DIR} --namespace default"
                   }
               }
           }
       }

       post {
           always {
               cleanWs()
           }
       }
   }
