pipeline {
    agent any
    stages {
        stage('compile') {
            steps {
                withMaven(maven:'MiMaven') {
                    sh 'mvn clean test'
                }
            }
        }
        stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    withMaven(maven:'MiMaven') {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
