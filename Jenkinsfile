pipeline {
    agent {
        label "jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'maven3'
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs
            }
        }
    }

        stages{
        stage("Checkout from SCM"){
            steps{
                git branch: 'main', url: 'https://github.com/elnady1bs/complete-prodcution-e2e-pipeline'
            }
        }
    }

}