pipeline {
    agent any
    tools{
        maven 'Maven'
        jdk 'jdk8'
    }
    environment{
        buildTag="${BUILD_NUMBER}"
    }
    stages{
        stage("Compile project"){
            steps{ sh 'mvn clean compile' }
        }
        stage("Run Tests"){
            steps{ sh "mvn test" }
        }
        stage("Build Project"){
            steps{ sh "mvn install -DskipTests" }
        }
        stage("Build Docker Image"){ 
            steps{
                sh 'docker build -t marianandrei/petclinic:${buildTag} .'
          }
        }
        stage("Push Docker Image"){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USER')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASSWORD'
                    sh 'docker push marianandrei/petclinic:${buildTag}'
                    }
                }
            }
        }
    }
}

