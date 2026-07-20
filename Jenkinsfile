pipeline {
    agent any

    tools {
        jdk 'java-17'
        maven 'maven'
    }

    environment {
        IMAGE_NAME = "VBDharaneesha/devopsproject-md:${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'test',
                    url: 'https://github.com/VBDharaneesha/DevopsProject-MD.git'
            }
        }

        stage('Verify Tools') {
            steps {
                sh '''
                    echo "JAVA_HOME=$JAVA_HOME"
                    java -version
                    mvn -version
                '''
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                     printenv
                     docker build -t ${IMAGE_NAME} .
                '''
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                    docker rm -f DevopsProject-MD-demo || true

                    docker run -d \
                        --name DevopsProject-MD-demo \
                        -p 9000:8080 \
                        ${IMAGE_NAME}
                '''
            }
        }

        stage('Docker Push to Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                    sh 'docker push ${IMAGE_NAME}'
                }
            }
        }
    }
}
