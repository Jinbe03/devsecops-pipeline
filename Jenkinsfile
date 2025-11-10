pipeline {
    // Usamos un agente Docker con Python
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
                sh 'pip install -r requirements.txt'
                sh 'bandit -r . || true'
            }
        }
    }
}
