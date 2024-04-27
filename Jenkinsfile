pipeline {
    agent any
   tools {
        maven 'maven-3'
    }
    stages {
        stage('Clean') {
            steps {
               cleanWs()
            }
        }
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/spring-projects/spring-petclinic'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean verify'
            }
        }
    }
    
    post {
        success {
            slackSend color: "good", message: "Pipeline pet-clinic-build succed - tag: ${env.BUILD_TAG}, url: ${env.BUILD_URL}"
        }
        unstable {
            slackSend color: "warning", message: "Pipeline pet-clinic-build unstable"
        }
        failure {
            slackSend color: "danger", message: "Pipeline pet-clinic-build FAILURE"
        }
        always {
            junit allowEmptyResults: true, testResults: '**/target/surefire-reports/*.xml'
        }
    }
}
