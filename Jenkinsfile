pipeline {
    agent any

    // Solo actuar sobre main
    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Linting') {
            steps {
                sh 'npm run lint'
            }
        }

        stage('Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
    }

    post {
        failure {
            // Notificar al equipo que main está roto
            mail to: 'equipo@tuempresa.com',
                 subject: "❌ Pipeline falló en main - ${env.GIT_COMMIT}",
                 body: "El commit ${env.GIT_COMMIT} de ${env.GIT_COMMITTER_NAME} rompió el pipeline.\n\nRevisa: ${env.BUILD_URL}"
        }
        success {
            echo '✅ Main estable'
        }
    }
}