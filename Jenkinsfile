pipeline {
    agent {
        label "Jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3' // Ensure 'Maven3' matches the name in your Jenkins configuration
    }
    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "elnady1bs"
        DOCKER_PASS = 'Hamza@#23'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
        JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")

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
                stage("Build & Push Docker Image") {
                    steps {
                        script {
                             docker.withRegistry('',DOCKER_PASS) {
                                docker_image = docker.build "${IMAGE_NAME}"
                            }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }

        }
    }
}
