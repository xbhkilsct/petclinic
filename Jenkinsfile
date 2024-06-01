pipeline {
    agent any
    environment {
        DOCKER_HUB_USERNAME = 'gown5914'
        DOCKER_HUB_REPOSITORY = 'petclinic'
        TAG = "${env.BUILD_NUMBER}"
        DOCKER_IMAGE = "${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPOSITORY}:${TAG}"
    }

    tools {
        maven 'maven 3.9.7'
        jdk 'jdk8-412'
    }

    stages {
        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Install') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}"
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(
                        credentialsId: 'docker-credentials',
                        usernameVariable: 'DOCKER_HUB_USERNAME',
                        passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                    sh "docker login -u ${DOCKER_HUB_USERNAME} -p ${DOCKER_HUB_PASSWORD}"
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }
    }
}
