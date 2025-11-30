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
                sh "docker build -t minamagdy11/sysadmin-java:v${BUILD_NUMBER} ."
            }
        }
        // stage('Push Docker Image') {
        //     steps {
        //         withCredentials([string(credentialsId: 'docker-username', variable: 'DOCKER_USERNAME'), string(credentialsId: 'docker-password', variable: 'DOCKER_PASSWORD')]) {
        //             sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
        //         }
        //         sh 'docker push minamagdy11/sysadmin-java:v1'
        //     }
        // }
        stage("Deploy App"){
            steps{
                sh "docker run -d -p 8090:8090 --name java minamagdy11/sysadmin-java:v${BUILD_NUMBER}"
            }
        }
    }
}
