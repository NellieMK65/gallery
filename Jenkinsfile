pipeline {
    agent any

    environment {
        PRODUCTION = "mongodb+srv://nelsonmuriithi:rFaA6yMQ9A9QXFDF@cluster0.oaihr.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0"
        DEVELOPMENT = "mongodb+srv://nelsonmuriithi:rFaA6yMQ9A9QXFDF@cluster0.oaihr.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0"
        TEST = "mongodb+srv://nelsonmuriithi:rFaA6yMQ9A9QXFDF@cluster0.oaihr.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0"

        EMAIL_SUBJECT_SUCCESS = "Gallery Project Build: ${env.JOB_NAME}:${env.BUILD_NUMBER} - Success"
        EMAIL_SUBJECT_FAILURE = "Gallery Project Build: ${env.JOB_NAME}:${env.BUILD_NUMBER} - Failed"
        EMAIL_RECEPIENT = "nelson.muriithi@moringaschool.com"
        EMAIL_BODY = """
            <html>
                <body>
                    <p>Build Details:</p>
                    <ul>
                        <li>Job Name: <b>${env.JOB_NAME}</b></li>
                        <li>Build Number: <b>${env.BUILD_NUMBER}</b></li>
                        <li>Build Status: <b>${currentBuild.currentResult}</b></li>
                    </ul>
                    <p>View build details <a href="${env.BUILD_URL}">here</a>.</p>
                </body>
            </html>
        """
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying Application'
                    sh 'curl -X POST https://api.render.com/deploys/rnd_EF1kVXvExwy9QOU527inPdaA5yXo'
                }
            }
        }
    }

    post {
        success {
            slackSend(
                channel: '#gallery-project',
                color: 'good',
                message: "Build ${currentBuild.fullDisplayName} completed successfully! URL: https://github.com/NellieMK65/gallery"
            )
            emailext(
                attachLog: true,
                body: EMAIL_BODY,
                subject: EMAIL_SUBJECT_SUCCESS,
                to: EMAIL_RECEPIENT
            )
        }
        failure {
            slackSend(
                channel: '#gallety-project',
                color: 'bad',
                message: "Build ${currentBuild.fullDisplayName} failed! Please check the logs for details."
            )
            emailext(
                attachLog: true,
                body: EMAIL_BODY,
                subject: EMAIL_SUBJECT_FAILURE,
                to: EMAIL_RECEPIENT
            )
        }
        always {
            cleanWs()
        }
    }
}
