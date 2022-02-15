pipeline {
    agent none

    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Maven') {
            agent {
                docker {
                     image 'maven:3.8.1-adoptopenjdk-11'
                     args '-v /root/.m2:/root/.m2'
                }
            }
        }
        stage('Node') {
            agent {
                docker {
                     image 'node:lts-bullseye-slim'
                     args '-p 3000:3000'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                // sh 'npm test'
                // sh 'npx percy exec -- mvn test'
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}