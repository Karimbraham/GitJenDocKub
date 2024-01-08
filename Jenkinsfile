pipeline{
    agent{
        label any
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
        APP_NAME = "gitjendockub"
        RELEASE = "1.0.0"
        DOCKER_USER = "karimbr"
        DOCKER_PASS = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"

    }

    stages{
        stage("Cleanup Workspace"){
            agent{
                    label "jenkins-agent"
                 }
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            agent{
                    label "jenkins-agent"
                 }
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Karimbraham/GitJenDocKub'
            }

        }

        stage("Build Application"){
            agent{
                    label "jenkins-agent"
                 }
            steps {
                sh "mvn clean package"
            }

        }

        stage("Test Application"){
            agent{
                    label "jenkins-agent"
                 }
            steps {
                sh "mvn test"
            }

        }
        
        stage("Build & Push Docker Image") {
            agent{
                    label "build-in"
                 }
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }

        }
    }

}
