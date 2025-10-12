pipeline {
  agent any

  stages {
    stage('Clone Repository') {
      steps {
        git url: 'https://github.com/<your-github-username>/flask-sample-webapp.git', branch: 'main'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t flask-demo-app .'
      }
    }

    stage('Run Container') {
      steps {
        sh 'docker stop flask-container || true'
        sh 'docker rm flask-container || true'
        sh 'docker run -d --name flask-container -p 5000:5000 flask-demo-app'
      }
    }
  }
}
