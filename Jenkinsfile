pipeline {
    // Run Script on server 1
    agent {
  label 'server1-labib'
}
    // Setup Tools Node
    tools {nodejs "nodejs"}

    // 
    stages {
        stage('Checkout SCM') {
           git branch: 'main',  
           url: 'https://github.com/khotimillabib/simple-apps.git'
        }
        stage('Build') {
            steps {
                sh '''cd apps
                npm install'''
            }
        }
        stage('Testing') {
            steps {
                sh '''cd apps
                npm test
                npm run test:coverage'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''cd apps
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.10.104:9000 \
                -Dsonar.login=63b607f278b53315bc25257df9b939fe26631fd9'''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose build
                docker compose up -d
                '''
            }
        }
    }
}