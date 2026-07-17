pipeline{
    agent any
    tools {
        jdk 'java-17'
        maven 'maven'
    }
    environment{
        IMAGE_NAME = "VBDharaneesha/DevopsProject-MD:${GIT_COMMIT}"
	JAVA_HOME = "/usr/lib/jvm/java-17-openjdk-amd64"
    }
    stages{
        stage('git-checkout'){
            steps{
                git url:'https://github.com/VBDharaneesha/DevopsProject-MD.git', branch: 'test'
            }
        }
        stage ('compile'){
            steps{
                sh '''
                    mvn compile
                '''
            }
        }
        stage('packaging'){
            steps{
                sh '''
                    mvn clean package
                '''
            }
        }
        stage('docker build'){
            steps{
                sh '''
                    docker build -t ${IMAGE_NAME} .
                '''
            }
        }
        stage('Docker-testing'){
            steps{
                sh '''
                    docker run -it -d --name DevopsProject-MD-demo -p 8081:8080 ${IMAGE_NAME}
                '''
            }
        }
    }
}
