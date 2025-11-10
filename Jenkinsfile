pipeline {
    agent { docker { image 'python:3.9-slim' } }

    stages {
        stage('Build') {
            steps {
                echo 'Construyendo el proyecto...'
            }
        }

        stage('Test') {
            steps {
                echo 'Ejecutando pruebas...'
                sh 'python3 app.py'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Instalando dependencias y ejecutando Bandit...'
                sh '''
                    pip install --no-cache-dir --upgrade pip
                    pip install --no-cache-dir -r requirements.txt
                    pip install --no-cache-dir bandit
                    bandit -r . -f txt -o bandit-report.txt || true
                    cat bandit-report.txt
                '''
            }
        }
    }

    post {
        always {
            echo 'Publicando reporte de seguridad...'
            archiveArtifacts artifacts: 'bandit-report.txt', allowEmptyArchive: true
        }
    }
}
