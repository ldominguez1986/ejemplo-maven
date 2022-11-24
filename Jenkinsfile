import groovy.json.JsonSlurperClassic

def jsonParse(def json) {
    new groovy.json.JsonSlurperClassic().parseText(json)
}
pipeline {
    agent any
    stages {
        stage("Paso 1: Compliar"){
            steps {
                script {
                env.STAGE='Paso 1: Compliar'
                sh "echo 'Compile Code!'"
                // Run Maven on a Unix agent.
                sh "./mvnw clean compile -e"
                }
            }
        }
        stage("Paso 2: Testear"){
            steps {
                script {
                env.STAGE='Paso 2: Testear'
                sh "echo 'Test Code!'"
                // Run Maven on a Unix agent.
                sh "./mvnw clean test -e"
                }
            }
        }
        stage("Paso 3: Build .Jar"){
            steps {
                script {
                env.STAGE='Paso 3: Build .Jar'
                sh "echo 'Build .Jar!'"
                // Run Maven on a Unix agent.
                sh "./mvnw clean package -e"
                }
            }
            post {
                //record the test results and archive the jar file.
                success {
                    archiveArtifacts artifacts:'build/*.jar'
                }
            }
        }
        stage("Paso 4: Análisis SonarQube"){
            steps {
                script {
                    env.STAGE='Paso 4: Analisis SonarQube'
                }
                withSonarQubeEnv('sonarqube') {
                    sh "echo 'Calling sonar Service in another docker container!'"
                    // Run Maven on a Unix agent to execute Sonar.
                    sh './mvnw clean verify sonar:sonar -Dsonar.projectKey=custom-project-key'
                }
            }
        }
        stage('Paso 5: Reportar en Slack') {
            steps {
                script{
                   env.STAGE='Paso 5: Reportar en Slack'
                   sh 'echo "Testins stage && Slack"'
                }
            }
        }
    }
    post {
        always {
            sh "echo 'fase always executed post'"
        }
		success{
					slackSend color: 'good', message: "[Luis Dominguez] [${JOB_NAME}] [${BUILD_TAG}] Ejecucion Exitosa", teamDomain: 'devopsusach20-lzc3526', tokenCredentialId: 'token-slack'
				}
				failure{
					slackSend color: 'danger', message: "[Luis Dominguez] [${env.JOB_NAME}] [${BUILD_TAG}] Ejecucion fallida en stage [${env.STAGE}]", teamDomain: 'devopsusach20-lzc3526', tokenCredentialId: 'token-slack'
				}
    }
}
