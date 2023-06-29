pipeline {
    agent any

    environment {
        M2_HOME = '/opt/apache-maven-3.9.3'
        PATH = "${env.M2_HOME}/bin:${env.PATH}"
        PROJECT_NAME = 'spring-petclinic'
        REPO_URL = '143.244.208.157:8083'
    }

    stages {
        stage('Setup') {
            steps {
                sh 'echo $PATH'
                sh 'mvn --version'
                // Nexus 3 Docker Repository Credentials
                script {
                    def dockerRegistryCredentials = credentials('nexus3-repository')
                    docker.withRegistry("http://${REPO_URL}", 'nexus3-repository') {
                    }
                }
            }
        }

        stage('Checkstyle') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    sh 'mvn checkstyle:checkstyle'
                }
            }
        }

        stage('Test') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    sh 'mvn test'
                }
            }
        }

        stage('Build') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    sh 'mvn clean install -Dmaven.test.skip=true'
                }
            }
        }

        stage('Build Image') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    script {
                        docker.build("${PROJECT_NAME}:${GIT_COMMIT[0..6]}", "-f Dockerfile.multi .")
                        docker.tag("${PROJECT_NAME}:${GIT_COMMIT[0..6]}", "${REPO_URL}/${PROJECT_NAME}:${GIT_COMMIT[0..6]}")
                    }
                    // sh 'docker build -t "${PROJECT_NAME}:${GIT_COMMIT[0..6]}" Dockerfile.multi .'
                    // sh ''
                    // script { 
                    //     docker tag "${PROJECT_NAME}:${GIT_COMMIT[0..6]}" "${REPO_URL}/${PROJECT_NAME}:${GIT_COMMIT[0..6]}"
                    // }
                }
            }
        }

        stage('Push') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    script {
                        docker.withRegistry("${REPO_URL}", 'nexus3-repository') {
                        docker.image("${PROJECT_NAME}:${GIT_COMMIT[0..6]}").push()
                        }
                    }
                    // script {
                    //     docker push "${REPO_URL}/${PROJECT_NAME}:${GIT_COMMIT[0..6]}"
                    // }
                }
            }
        }
    }

    post {
        always {
            // Clean up Maven artifacts after each build
            deleteDir()
        }
        success {
            // Clean up Docker image after successful push
            script {
                docker.image("${REPO_URL}/${PROJECT_NAME}:${GIT_COMMIT[0..6]}").remove()
            }
        }
        
        failure {
            // Clean up Docker image after failed push
            script {
                docker.image("${REPO_URL}/${PROJECT_NAME}:${GIT_COMMIT[0..6]}").remove()
            }
        }
    }
}
