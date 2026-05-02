pipeline {
    agent any

    stages {
        stage('Stop and Clean') {
            steps {
                sh 'docker-compose -f docker-compose-ci.yml down || true'
                
                cleanWs()
            }
        }

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Nayab-khalid/focusflow2-part2.git'
            }
        }

        stage('Run CI Container') {
            steps {
                sh 'docker-compose -f docker-compose-ci.yml up -d --build'
            }
        }

        stage('Verify Running') {
            steps {
                sh 'docker ps'
                sh 'sleep 10' // Give the app some time to start
            }
        }

        stage('Run Selenium Tests') {
            steps {
                sh '''
                export BASE_URL=http://localhost:4000
                npm run selenium
                '''
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Website is live at http://16.16.205.105:4000'
        }
    }
}
