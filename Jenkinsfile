pipeline {
    agent any

    tools {
        // Install the Maven version configured as "Maven" and add it to the path.
        maven "Maven"
    }

    stages {
        stage('Build Maven') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/TelieChris/TechConnectApp',
                        credentialsId: '89d5394e-ea16-48ab-bbaa-e898817b1738' // Replace with your actual credentials ID
                    ]]
                ])
                bat 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    bat 'docker build -t techconnect .'
                }
            }
        }
        stage('Push image to Docker Hub'){
            steps{
                script {
                    withCredentials([usernamePassword(credentialsId: 'DockerHubCredentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        bat '''
                            echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD% 
                        '''
                    }
                    bat 'docker tag techconnect 50604/techconnect:mytag'
                    bat 'docker login docker.io'
                    bat 'docker push 50604/techconnect:mytag'
                }
            }
        }
    }
}
