pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {

        stage('Stop and Clean') {
            steps {
                sh 'docker-compose -p focusflow2-part2 -f docker-compose-ci.yml down || true'
                cleanWs()
            }
        }

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Nayab-khalid/focusflow2-part2'
            }
        }

        stage('Start Application') {
            steps {
                sh 'docker-compose -p focusflow2-part2 -f docker-compose-ci.yml up -d --build'
                sh 'sleep 30'
            }
        }

        stage('Run Selenium Tests (Containerized)') {
            steps {
                sh '''
                docker run --rm \
                --network focusflow2-part2_default \
                -v $PWD:/app \
                -w /app \
                -e BASE_URL=http://focusflow-app:3000 \
                markhobson/node-chrome:latest \
                sh -c "node selenium_tests/test_focusflow.js"
                '''
            }
        }

        stage('Verify Running') {
            steps {
                sh 'docker ps'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: All tests passed!'

            mail to: 'nayab.khalid13@gmail.com, qasimalik@gmail.com',
                 subject: 'Jenkins SUCCESS: FocusFlow Tests Passed',
                 body: 'Pipeline executed successfully. All Selenium test cases passed and app is running.'
        }

        failure {
            echo 'FAILURE: Pipeline failed.'

            mail to: 'nayab.khalid13@gmail.com, qasimalik@gmail.com',
                 subject: 'Jenkins FAILURE: FocusFlow Pipeline',
                 body: 'Pipeline failed. Check Jenkins console output.'
        }
    }
}