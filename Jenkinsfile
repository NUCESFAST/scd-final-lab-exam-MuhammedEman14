pipeline {
    agent any

    environment {
        // Define environment variables for Docker Hub credentials
        DOCKERHUB_CREDENTIALS = credentials('22SEP2023') // Jenkins credentials ID
        DOCKERHUB_NAMESPACE = 'muhammedeman14'
        GIT_REPO_URL = 'https://github.com/NUCESFAST/scd-final-lab-exam-MuhammedEman14'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the GitHub repository
                git url: "${env.GIT_REPO_URL}", branch: 'main'
            }
        }

        stage('Install Dependencies') {
            parallel {
                stage('Install Frontend Dependencies') {
                    steps {
                        dir('client') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Install Auth Dependencies') {
                    steps {
                        dir('Auth') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Install Classrooms Dependencies') {
                    steps {
                        dir('Classrooms') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Install Event-Bus Dependencies') {
                    steps {
                        dir('event-bus') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Install Post Dependencies') {
                    steps {
                        dir('Post') {
                            sh 'npm install'
                        }
                    }
                }
            }
        }

        stage('Build Docker Images') {
            parallel {
                stage('Build Frontend Image') {
                    steps {
                        script {
                            docker.build("${DOCKERHUB_NAMESPACE}/frontend", 'client')
                        }
                    }
                }
                stage('Build Auth Image') {
                    steps {
                        script {
                            docker.build("${DOCKERHUB_NAMESPACE}/auth1", 'Auth')
                        }
                    }
                }
                stage('Build Classrooms Image') {
                    steps {
                        script {
                            docker.build("${DOCKERHUB_NAMESPACE}/classrooms1", 'Classrooms')
                        }
                    }
                }
                stage('Build Event-Bus Image') {
                    steps {
                        script {
                            docker.build("${DOCKERHUB_NAMESPACE}/event-bus1", 'event-bus')
                        }
                    }
                }
                stage('Build Post Image') {
                    steps {
                        script {
                            docker.build("${DOCKERHUB_NAMESPACE}/post1", 'Post')
                        }
                    }
                }
            }
        }

        stage('Push Docker Images') {
            parallel {
                stage('Push Frontend Image') {
                    steps {
                        script {
                            docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                                docker.image("${DOCKERHUB_NAMESPACE}/frontend").push()
                            }
                        }
                    }
                }
                stage('Push Auth Image') {
                    steps {
                        script {
                            docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                                docker.image("${DOCKERHUB_NAMESPACE}/auth1").push()
                            }
                        }
                    }
                }
                stage('Push Classrooms Image') {
                    steps {
                        script {
                            docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                                docker.image("${DOCKERHUB_NAMESPACE}/classrooms1").push()
                            }
                        }
                    }
                }
                stage('Push Event-Bus Image') {
                    steps {
                        script {
                            docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                                docker.image("${DOCKERHUB_NAMESPACE}/event-bus1").push()
                            }
                        }
                    }
                }
                stage('Push Post Image') {
                    steps {
                        script {
                            docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                                docker.image("${DOCKERHUB_NAMESPACE}/post1").push()
                            }
                        }
                    }
                }
            }
        }

        stage('Validate Deployment') {
            steps {
                script {
                    // Pull the images from Docker Hub
                    sh 'docker-compose pull'

                    // Start the containers
                    sh 'docker-compose up -d'

                    // Check the status of the containers
                    sh 'docker-compose ps'

                    // Validate the application (replace with actual validation steps)
                    sh 'curl -f http://localhost:6868 || exit 1'
                }
            }
        }
    }

    post {
        always {
            // Clean up the workspace
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
