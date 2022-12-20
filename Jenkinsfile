pipeline {
    agent any
    tools {
      maven 'MiMaven'
    }

    stages {
        stage('compile') {
            steps {
                sh 'mvn clean test'
            }
        }
        stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn sonar:sonar'
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
