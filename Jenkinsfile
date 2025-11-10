pipeline {
    agent { docker { image 'python:3.9-slim'; args '-u root' } }

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
                    set -e
                    pip install --no-cache-dir --upgrade pip
                    pip install --no-cache-dir -r requirements.txt
                    pip install --no-cache-dir pbr bandit
                    echo "✅ Dependencias instaladas correctamente"
                    bandit -r . -f txt -o bandit-report.txt || true
                    echo "📄 Resultado del escaneo con Bandit:"
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
