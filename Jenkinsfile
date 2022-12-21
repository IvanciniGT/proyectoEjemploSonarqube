pipeline {
    agent any
    tools {
      maven 'MiMaven'
    }

    stages {
        stage('Compilacion') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Compilacion de pruebas unitarias') {
            steps {
                sh 'mvn test-compile'
            }
        }
        stage('Pruebas') {
            stages{
                stage('Ejecución de pruebas unitarias') {
                    steps {
                        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                            sh 'mvn test'
                        }
                    }
                    post{
                        always {
                            sh 'mvn jacoco:report'
                        }
                    }
                }
                stage('Lanzar el analisis de Sonarqube') {
                    steps {
                        withSonarQubeEnv('sonarqube') {
                            sh 'mvn sonar:sonar'
                        }
                    }
                }
                stage("Esperar la respuesta del Sonarqube") {
                    steps {
                        timeout(time: 1, unit: 'HOURS') {
                            waitForQualityGate abortPipeline: true
                        }
                    }
                }
            }
            post{
                failure {
                    error 'Fallo en los test unitarios'
                }
            }
        }
    }
}
