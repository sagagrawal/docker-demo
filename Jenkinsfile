pipeline{
    agent any
    
    environment {
        imagename = "sagagrawal/demorepo-sagar04"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    stages{
        stage('Cloning Git') {
            steps {
                git([url: 'https://github.com/sagagrawal/docker-demo.git', branch: 'master'])
            }
        }
        
        stage('Building image') {
            steps{
                    script {
                    dockerImage = docker.build imagename
                }
            }
        }

        stage('Deploy Image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push("$BUILD_NUMBER")
                        dockerImage.push('latest')
                    }
                }
            }
        }
        
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $imagename:$BUILD_NUMBER"
                sh "docker rmi $imagename:latest"
            }
        }
    }
}