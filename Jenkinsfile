pipeline {
    agent none

    environment {
        M2_HOME = '/opt/apache-maven-3.9.3'
        PATH = "${env.M2_HOME}/bin:${env.PATH}"
        PROJECT_NAME = 'spring-petclinic'
        REPO_URL = '143.244.208.157:8083'
    }

    stages {
        stage('Setup') {
            agent agent1
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
            agent agent1
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    sh 'mvn checkstyle:checkstyle'
                }
            }
        }

        stage('Test') {
            agent agent1
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    sh 'mvn test'
                }
            }
        }

        stage('Build') {
            agent agent1
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    sh 'mvn clean install -Dmaven.test.skip=true'
                }
            }
        }

        stage('Build Image') {
            agent agent1
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    sh "docker build -t ${PROJECT_NAME}:${GIT_COMMIT.take(7)} -f Dockerfile.multi ."
                    sh "docker tag ${PROJECT_NAME}:${GIT_COMMIT.take(7)} ${REPO_URL}/${PROJECT_NAME}:${GIT_COMMIT.take(7)}"
                }
            }
        }

        stage('Push') {
            parallel {
                stage('Docker Push') {
                    agent agent1
                    steps {
                        catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                            sh "docker push ${REPO_URL}/${PROJECT_NAME}:${GIT_COMMIT.take(7)}"
                        }
                    }
                }
                stage('Maven Deploy') {
                    agent agent1
                    steps {
                        catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                            sh 'mvn deploy -Dmaven.test.skip=true'
                        }
                    }
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
                sh "docker rmi -f ${PROJECT_NAME}:${GIT_COMMIT.take(7)}"
            }
        }
        failure {
            // Clean up Docker image after failed push
            script {
                sh "docker rmi -f ${PROJECT_NAME}:${GIT_COMMIT.take(7)}"
            }
        }
    }
}
