pipeline {
    agent any

    environment {
        // Variables de entorno disponibles en todo el pipeline
        REPO_NAME = "${env.GIT_URL.tokenize('/').last().replace('.git', '')}"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Clonando rama: ${env.BRANCH_NAME}"
                checkout scm
            }
        }

        stage('Instalar dependencias') {
            steps {
                sh '''
                    # Ajusta según tu stack (Node, Python, Java, etc.)
                    npm install        # Node.js
                    # pip install -r requirements.txt   # Python
                    # mvn dependency:resolve            # Java/Maven
                '''
            }
        }

        stage('Linting') {
            steps {
                sh '''
                    npm run lint
                    # flake8 .          # Python
                '''
            }
        }

        stage('Tests unitarios') {
            steps {
                sh '''
                    npm test -- --coverage
                    # pytest --junitxml=results.xml   # Python
                '''
            }
            post {
                always {
                    // Publicar resultados de tests en Jenkins
                    junit '**/test-results/*.xml'
                }
            }
        }

        stage('Build') {
            steps {
                sh '''
                    npm run build
                    # mvn package -DskipTests   # Java
                '''
            }
        }

        stage('Análisis de seguridad (opcional)') {
            steps {
                sh '''
                    # Ejemplo con npm audit
                    npm audit --audit-level=high
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completado exitosamente'
            // Aquí puedes agregar notificaciones (Slack, email, etc.)
        }
        failure {
            echo '❌ Pipeline falló - revisar logs'
        }
        always {
            // Limpiar workspace al terminar
            cleanWs()
        }
    }
}