pipeline {
    agent any
    tools {
       maven 'M2_HOME'
    }
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                echo 'hi'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package  -DskipTests'
            }
        }
        stage('sonarqube checks') {
            steps {
                script {
                withSonarQubeEnv(installationName: 'sonarqubecloud', credentialsId: 'Sonarscanneroncloud') {
                 sh 'mvn clean package sonar:sonar'
                 }
                    timeout(time: 1, unit: 'HOURS') {
                       def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                       error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                }
            }
        }
    }
}
}
