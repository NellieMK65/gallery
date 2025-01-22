pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    bat 'npm install'
                }
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // echo 'Deploying Application'
                    bat 'curl -X POST https://api.render.com/deploy/srv-cu8abfij1k6c739sge30?key=iBDdzLy5hU0'
                }
            }
        }
    }

    post {
        success {
            slackSend(
                channel: '#gallery-project',
                color: 'good',
                message: "Build ${currentBuild.fullDisplayName} completed successfully! URL: https://gallery-j6w7.onrender.com/"
            )
        }
        failure {
            slackSend(
                channel: '#gallery-project',
                color: 'bad',
                message: "Build ${currentBuild.fullDisplayName} failed! Please check the logs for details."
            )
        }
        always {
            cleanWs()
        }
    }
}
