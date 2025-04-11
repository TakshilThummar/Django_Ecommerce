pipeline {
    agent any

    stages {
        
        stage('git pull') {
            steps {
                git url: 'https://github.com/TakshilThummar/Django_Ecommerce.git', branch: 'master'
            }
        }
        
        stage ('Docker container stop') {
            steps {
                script {
                    def containerExists = sh(script: "sudo docker ps -q --filter 'name=django-ecommerce'", returnStdout: true).trim()
                    if (containerExists) {
                        echo 'Docker container exists, stopping...'
                        sh 'sudo docker stop django-ecommerce'
                    } else {
                        echo 'Docker container does not exist, skipping stop stage'
                    }
                }
            }
        }
        
        stage ('Docker container delete') {
            steps {
                script {
                    def containerExists = sh(script: "sudo docker ps -a -q --filter 'name=django-ecommerce'", returnStdout: true).trim()
                    if (containerExists) {
                        echo 'Docker container exists, deleting...'
                        sh 'sudo docker rm django-ecommerce'
                    } else {
                        echo 'Docker container does not exist, skipping delete stage'
                    }
                }
            }
        }
        
        stage ('Docker image delete') {
            steps {
                script {
                    def imageExists = sh(script: "sudo docker images -q django-ecommerce", returnStdout: true).trim()
                    if (imageExists) {
                        echo 'Docker image exists, deleting...'
                        sh 'sudo docker image rm django-ecommerce'
                    } else {
                        echo 'Docker image does not exist, skipping delete stage'
                    }
                }
            }
        }
        
        stage('create docker image') {
            steps {
                sh 'sudo docker build -t django-ecommerce .'
                echo 'docker image created'
            }
        }
        
        stage('create docker container') {
            steps {
                sh 'sudo docker run -td --name django-ecommerce -p 8000:8000 django-ecommerce:latest'
                echo 'docker container created'
            }
        }
        
    }
}
