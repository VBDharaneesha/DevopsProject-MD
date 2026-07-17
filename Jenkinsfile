pipeline {
    agent any
    tools {
        jdk 'java-17'
        maven 'maven'
    }
    environment {
        IMAGE_NAME = "VBDharaneesha/DevopsProject-MD:${GIT_COMMIT}"
    }
    stages {
        stage('git-checkout') {
            steps {
                git url: 'https://github.com/VBDharaneesha/DevopsProject-MD.git', branch: 'test'
            }
        }
        stage('compile') {
            steps {
                /* 
                   We explicitly bind the paths inside the shell block using the tool homes 
                   that Jenkins configures automatically out of your tools block.
                */
                sh '''
                    export JAVA_HOME="${tool 'java-17'}"
                    export PATH="${tool 'maven'}/bin:$JAVA_HOME/bin:$PATH"
                    mvn compile
                '''
            }
        }
        stage('packaging') {
            steps {
                sh '''
                    export JAVA_HOME="${tool 'java-17'}"
                    export PATH="${tool 'maven'}/bin:$JAVA_HOME/bin:$PATH"
                    mvn clean package
                '''
            }
        }
        stage('docker build') {
            steps {
                sh '''
                    docker build -t ${IMAGE_NAME} .
                '''
            }
        }
        stage('Docker-testing') {
            steps {
                sh '''
                    docker run -it -d --name DevopsProject-MD-demo -p 8081:8080 ${IMAGE_NAME}
                '''
            }
        }
    }
}
