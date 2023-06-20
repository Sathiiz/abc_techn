pipeline {
    agent {
        label 'edureka'
    }
    tools {
        maven 'MyMaven'
    }
    stages {      
        stage ('1.Complie Code') {
            steps {
                sh 'mvn compile'
        }
        }
        stage ('2.Test Code') {
            steps {
                sh 'mvn test'
        }
            post{
            success{
                junit 'target/surefire-reports/*.xml'
            }
        }
        }
        stage ('3.Package Code') {
            steps {
                sh 'mvn clean package'
                sh 'mvn clean install'
        }
        }
        stage ('4.Build and Push Docker Image') {
            steps {
                sh 'docker build -t sathiz/$JOB_NAME:$BUILD_NUMBER .'
            withCredentials([usernamePassword(credentialsId: 'aa052daa-7a16-4c8a-8694-5b406826bd7e', passwordVariable: 'docker_pwd', usernameVariable: 'docker_ID')]) {
                sh "echo '${docker_pwd}' | docker login -u ${docker_ID} --password-stdin"
            }
                sh 'docker push sathiz/$JOB_NAME:$BUILD_NUMBER'
            }
        }       
    }
}
