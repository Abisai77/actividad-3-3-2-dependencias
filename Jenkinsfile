pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo '--- ETAPA 1: BUILD (Preparando entorno Python y pipenv) ---'
                sh 'python3 --version'
                sh 'pipenv --version'
            }
        }
        stage('Test') {
            steps {
                echo '--- ETAPA 2: TEST (Ejecutando pruebas unitarias) ---'
                sh 'pipenv install'
                sh 'pipenv run python -c "import app; assert app.app is not None; print(\'TEST UNITARIO APROBADO\')"'
            }
        }
        stage('Analyze') {
            steps {
                echo '--- ETAPA 3: ANALYZE (Análisis de calidad y seguridad con SonarQube) ---'
                script {
                    def scannerHome = tool 'SonarScanner'
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=actividad-3-3-2-dependencias \
                            -Dsonar.projectName="Actividad 3.3.2 Dependencias Seguras" \
                            -Dsonar.sources=. \
                            -Dsonar.python.version=3.11
                        """
                    }
                }
            }
        }
        stage('Dependency Management') {
            steps {
                echo '--- ETAPA 4: DEPENDENCY MANAGEMENT (Instalación y actualización segura con pipenv) ---'
                sh 'pipenv update'
                sh 'pipenv graph'
            }
        }
        stage('Deploy') {
            steps {
                echo '--- ETAPA 5: DEPLOY (Desplegando aplicación en entorno de prueba controlado) ---'
                sh 'pkill -f "python.*app.py" || true'
                sh 'JENKINS_NODE_COOKIE=dontKillMe nohup pipenv run python app.py > app.log 2>&1 &'
                sh 'sleep 3'
                sh 'curl -s http://127.0.0.1:5000/'
            }
        }
    }
}
