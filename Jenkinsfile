// pipeline {
//     // agent any
//     agent {
//         docker {
//             image 'maven:latest'
//             args '-v /your/local/maven/repository:/root/.m2/repository'
//         }
//     }
//     options {
//         // Configure the SCM checkout behavior
//         skipDefaultCheckout(true)
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout([
//                     $class: 'GitSCM',
//                     branches: [[name: '*/main']],  // Specify the branch to build
//                     userRemoteConfigs: [[url: 'https://github.com/MiguelABFlores/spring-petclinic']],
//                     extensions: [
//                         [$class: 'CloneOption', depth: 1, noTags: false, reference: '', shallow: true],
//                         [$class: 'CleanBeforeCheckout']
//                     ]
//                 ])
//             }
//         }
//         stage('Build') {
//             steps {
//                 // Run Maven build

//             // sh 'mvn clean install'
//             }
//         }
//         stage('Test') {
//             steps {
//                 // Run Maven tests
//                 sh 'mvn test'
//             }
//         }
//     // Add more stages as needed
//     }
// }
def skipRemainingStages = false
pipeline {
    agent any

    stages {
        stage('Stage 1') {
            steps {
                script {
                    skipRemainingStages = true

                    println "skipRemainingStages = ${skipRemainingStages}"
                }
            }
        }
        stage('Stage 2') {
            when {
                expression {
                    !skipRemainingStages
                }
            }
            steps {
                script {
                    println 'This text wont show up....'
                }
            }
        }
        stage('Stage 3') {
            when {
                expression {
                    !skipRemainingStages
                }
            }
            steps {
                script {
                    println 'This text wont show up....'
                }
            }
        }
    }
}
