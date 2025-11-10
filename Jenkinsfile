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
                script {
                    docker.image('python:3.9-slim').inside('--user=root') {
                        sh '''
                            pip install -r requirements.txt
                            bandit -r . -f txt -o bandit-report.txt || true
                            cat bandit-report.txt
                        '''
                    }
                }
            }
        }
    }
}
