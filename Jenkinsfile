pipeline {
  agent any

  environment {
    DOCKER_IMAGE = 'flask-demo-app'
    DOCKER_TAG = 'v1'
    DOCKER_USERNAME = 'vedantgogawale123'
    FULL_IMAGE_NAME = "${DOCKER_USERNAME}/${DOCKER_IMAGE}:${DOCKER_TAG}"
  }

  stages {
    stage('Clone Repository') {
      steps {
        git url: 'https://github.com/Vedant-Gogawale/jenkins-repo.git', branch: 'main'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh "docker build -t ${DOCKER_IMAGE} ."
      }
    }

    stage('Tag Docker Image') {
      steps {
        sh "docker tag ${DOCKER_IMAGE} ${FULL_IMAGE_NAME}"
      }
    }

    stage('Login to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh "echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin"
        }
      }
    }

    stage('Push to Docker Hub') {
      steps {
        sh "docker push ${FULL_IMAGE_NAME}"
      }
    }

    stage('Run Container') {
      steps {
        sh 'docker stop flask-container || true'
        sh 'docker rm flask-container || true'
        sh "docker run -d --name flask-container -p 5000:5000 ${FULL_IMAGE_NAME}"
      }
    }
  }
}
