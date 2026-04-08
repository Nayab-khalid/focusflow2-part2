pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Nayab-khalid/focusflow2-part2.git'
            }
        }

        stage('Run CI Container') {
            steps {
                sh '''
                docker-compose -f docker-compose-ci.yml down || true
                docker-compose -f docker-compose-ci.yml up -d
                '''
            }
        }

        stage('Verify Running') {
            steps {
                sh 'docker ps'
            }
        }

    }
}
