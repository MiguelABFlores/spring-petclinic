pipeline {
    agent {
        label 'agent1' // Specify the label of the agent1 node
    }

    stages {
        stage('Checkstyle') {
            agent {
                agent1 {
                    image 'maven:latest' // Use Maven image as the base image for the container
                    args '-v $HOME/.m2:/root/.m2' // Mount Maven cache directory as a volume
                }
            }
            steps {
                sh 'mvn checkstyle:checkstyle'
                archiveArtifacts artifacts: 'target/checkstyle-result.xml', onlyIfSuccessful: false
            }
        }

        stage('Test') {
            agent {
                agent1 {
                    image 'maven:latest' // Use Maven image as the base image for the container
                    args '-v $HOME/.m2:/root/.m2' // Mount Maven cache directory as a volume
                }
            }
            steps {
                sh 'mvn test'
            }
        }

        stage('Build') {
            agent {
                agent1 {
                    image 'maven:latest' // Use Maven image as the base image for the container
                    args '-v $HOME/.m2:/root/.m2' // Mount Maven cache directory as a volume
                }
            }
            steps {
                sh 'mvn package -DskipTests'
            }
        }

// stage('Create Docker Image - Merge Request') {
//     environment {
//         GIT_COMMIT_SHORT = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
//     }
//     agent {
//         label 'agent1' // Use the agent1 node for running Docker commands
//     }
//     steps {
//         sh 'docker build -t mr/${env.GIT_COMMIT_SHORT} -f Dockerfile .'
//         sh 'docker tag mr/${env.GIT_COMMIT_SHORT} nexus/repository/mr:${env.GIT_COMMIT_SHORT}'
//         withCredentials([usernamePassword(credentialsId: 'nexus-credentials', passwordVariable: 'NEXUS_PASSWORD', usernameVariable: 'NEXUS_USERNAME')]) {
//             sh 'mvn deploy -Ddocker.image=nexus/repository/mr:${env.GIT_COMMIT_SHORT} -Ddocker.username=${NEXUS_USERNAME} -Ddocker.password=${NEXUS_PASSWORD}'
//         }
//     }
// }

    // post {
    //     success {
    //         stage('Create Docker Image - Main Branch') {
    //             agent {
    //                 label 'agent1' // Use the agent1 node for running Docker commands
    //             }
    //             steps {
    //                 sh 'docker build -t main -f Dockerfile .'
    //                 sh 'docker tag main nexus/repository/main:latest'
    //                 withCredentials([usernamePassword(credentialsId: 'nexus-credentials', passwordVariable: 'NEXUS_PASSWORD', usernameVariable: 'NEXUS_USERNAME')]) {
    //                     sh 'mvn deploy -Ddocker.image=nexus/repository/main:latest -Ddocker.username=${NEXUS_USERNAME} -Ddocker.password=${NEXUS_PASSWORD}'
    //                 }
    //             }
    //         }
    //     }
    // }
    }
}
