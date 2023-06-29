pipeline {
    agent any

    environment {
        M2_HOME = '/opt/apache-maven-3.9.3'
        PATH = "${env.M2_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Setup') {
            steps {
                sh 'echo $PATH'
                sh 'mvn --version'
                // Nexus 3 Docker Repository Credentials
                script {
                    def dockerRegistryCredentials = credentials('nexus3-repository')
                    docker.withRegistry('143.244.208.157:8083', 'nexus3-repository') {
                        // Perform Docker operations (e.g., build, push)
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

        // stage('Build Image') {
        //     steps {
        //         catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
        //             sh 'docker build -t spring-petclinic:${GIT_COMMIT:0:7} .'
        //             sh 'docker tag spring-petclinic:${GIT_COMMIT:0:7} mr/spring-petclinic:${GIT_COMMIT:0:7}'
        //             sh 'docker push mr/spring-petclinic:${GIT_COMMIT:0:7}'
        //         }
        //     }
        // }

        // stage('Push') {
        //     steps {
        //         catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {

        //         }
        //     }
        // }
    }

    post {
        always {
            // Clean up Maven artifacts after each build
            deleteDir()
        }
    }
}
