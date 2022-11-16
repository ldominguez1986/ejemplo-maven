import groovy.json.JsonSlurperClassic

def jsonParse(def json) {
    new groovy.json.JsonSlurperClassic().parseText(json)
}
pipeline {
    agent any
    stages {
        stage("Saludar"){
            steps {
                script {
                sh "echo 'Hello, World Usach!'"
                }
            }
        }
        stage("Sonar: Análisis SonarQube"){
            steps {
                sh "echo 'Calling sonar Service in another docker container!'"
                // Run Maven on a Unix agent to execute Sonar.
                sh './mvnw clean verify sonar:sonar -Dsonar.projectKey=git-ejemplo-maven-sonar -Dsonar.host.url=http://192.168.1.19:3001 -Dsonar.login=sqp_c9a8be1ad066cd904681baac31834a6239f0b374'
            }
        }
    }
    post {
        always {
            sh "echo 'fase always executed post'"
        }
        success {
            sh "echo 'fase success'"
        }
        failure {
            sh "echo 'fase failure'"
        }
    }
}
