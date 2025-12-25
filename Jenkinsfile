pipeline {
    agent { label 'server1-teguh||dev' }

    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/teguhbr/simple-apps.git'
            }
        }
        
        stage('Build') {
            steps {
                sh'npm install'
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cp app/.env .
                APP_PORT=5001 npm test
                APP_PORT=5001 npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
                cd app
                sonar-scanner \
                -Dsonar.projectKey=simple-apps2 \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.2.121:9000 \
                -Dsonar.token=sqp_7479dbaab5def86a11b37f9811fc57b865a4e3b7
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
        
        stage('Backup') {
            steps {
                 sh 'docker compose push' 
            }
        }
    }
}