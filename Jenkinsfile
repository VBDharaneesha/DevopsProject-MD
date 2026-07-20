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

        stage('Docker Hub Login') {
            environment {
                DOCKER_CREDS = credentials('docker-hub-creds')
            }
            steps {
                // Using stage-level environment credentials handles masking and keeps the shell call secure
                sh 'echo "$DOCKER_CREDS_PSW" | docker login -u "$DOCKER_CREDS_USR" --password-stdin'
            }
        }

        stage('Docker Hub Push') {
            steps {
                sh 'docker push ${IMAGE_NAME}'
            }
        }
    }
}
