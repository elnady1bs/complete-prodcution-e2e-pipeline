pipeline {
    agent {
        label "Jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3' // Ensure 'Maven3' matches the name in your Jenkins configuration
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs() // Ensure this step is invoked with parentheses
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', url: 'https://github.com/elnady1bs/complete-prodcution-e2e-pipeline'
            }
        }
        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

        }

        stage("Test Application"){
            steps {
                sh "mvn test"
            }

        }
        stage("Sonarqube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
                        sh "mvn sonar:sonar"
                    }
                }
            }

        }
        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }
            }

        }
    }
}
