pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    // Build the project
                    echo 'Building...'
                    git branch: 'main', url: 'https://github.com/rizki2232/acss.git'
                    sh 'cp .env.example .env'
                    sh 'docker compose down'
                    sh 'docker compose up -d --build'
                    sh 'docker-compose exec -T php composer install'
                    sh 'docker-compose exec -T php npm install'
                    sh 'docker-compose exec -T php php artisan key:generate'
                    sh 'docker-compose exec -T php php artisan migrate:fresh --seed'
                    sh 'docker-compose exec -T php npm run build'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    // Run tests
                    echo 'Testing...'
                    sh './vendor/bin/phpunit'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Deploy the project
                    echo 'Deploying...'
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful. Visit: http://<ip-server>:$PORT"
        }
        failure {
            echo "❌ Deployment failed! Check logs for details."
            sh 'docker compose logs'
        }
    }
}
