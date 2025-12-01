@Library('sysadmin')_

pipeline {
    agent {
        label 'java-agent-01'
    }
    tools {
        maven 'maven-352'
        jdk 'java-11'
    }
    stages {
        stage('Build App') {
            steps {
                sh 'java --version'
                sh 'mvn package install -DskipTests'
            }
        }
        stage('Archieve App') {
            steps {
                archiveArtifacts artifacts: '**/*.jar'
            }
        }
        stage('Build Docker Image') {
            steps {
                script{
                    def dockerClass = new edu.iti.docker()
                    dockerClass.build("minamagdy11/sysadmin-java", "v${BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script{
                    def dockerClass = new edu.iti.docker()
                    withCredentials([string(credentialsId: 'docker-username', variable: 'DOCKER_USERNAME'), string(credentialsId: 'docker-password', variable: 'DOCKER_PASSWORD')]) {
                    dockerClass.login("${DOCKER_USERNAME}", "${DOCKER_PASSWORD}")
                    }
                    dockerClass.push("minamagdy11/sysadmin-java", "v${BUILD_NUMBER}")
                }
            }
        }
    }
}
