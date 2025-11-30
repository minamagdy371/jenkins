pipeline {
    agent {
        label 'java-agent-01'
    }
    tools {
        maven 'maven-352'
        jdk 'java-11'
    }
    stages {
        stage('checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Hassan-Eid-Hassan/cicd-lab2.git'
            }
        }
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
                sh 'docker build -t minamagdy11/sysadmin-java:v1 .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'docker-username', variable: 'DOCKER_USERNAME'), string(credentialsId: 'docker-password', variable: 'DOCKER_PASSWORD')]) {
                    sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                }
                sh 'docker push minamagdy11/sysadmin-java:v1'
            }
        }
    }
}
