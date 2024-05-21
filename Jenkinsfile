pipeline {
    agent any

    tools {
        dockerTool 'docker-latest'
    }
    
    stages {
        stage('Test python') {
            steps {
                sh 'python main.py'
            }
        }

        stage('Test dfx') {
            steps {
                sh 'dfx'
            }
        }

        stage('Dfx start') {
            steps {
                sh 'dfx start &'
            }
        }

        stage('Dfx deploy') {
            steps {
                sh 'dfx deploy'
            }
        }

        stage('Building Docker Image') {
            steps {
                script {
                    sh(script: 'docker build -t back-image:latest .', label: 'Building image')
                }
            }
        }
        
        stage('Pushing Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "docker login -u $DOCKER_USER -p $DOCKER_PASSWORD"
                    sh 'docker tag back-image:latest genzoo/back-image:latest'
                    sh 'docker push genzoo/back-image:latest'
                }
            }
        }
    }

    post {
        always {
            script {
                sh 'docker logout'
            }
        }
    }
}
