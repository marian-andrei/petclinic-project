pipeline {
    agent any
    tools{
        maven 'Maven'
        jdk 'jdk8'
    }
    stages{
        stage("Compile project"){
            steps{ sh 'mvn clean compile' }
        }
        stage("Run Tests"){
            steps{ sh "mvn test" }
        }
        stage("Build Project"){
            steps{ sh "mvn clean install" }
        }
        stage("Build Docker Image"){ 
            steps{
                sh 'docker build -t marianandrei/petclinic:$env.BUILD_NUMBER .'
          }
        }
        stage("Push Docker Image"){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: "docker-creds", usernameVariable:"DOCKER_USER", passwordVariable: "DOCKER_PASSWORD" )])
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASSWORD'
                    sh 'docker push marianandrei/petclinic:$env.BUILD_NUMBER'
                }
            }
        }

    }
}