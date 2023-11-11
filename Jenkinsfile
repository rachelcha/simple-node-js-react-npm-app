// pipeline {
//     agent {
//         docker {
//             image 'node:20.9.0-alpine3.18'
//             args '-p 3000:3000'
//         }
//     }
//     stages {
//         stage('Build') {
//             steps {
//                 sh 'npm install'
//             }
//         }
//         stage('Test') {
//             steps {
//                 sh './jenkins/scripts/test.sh'
//             }
//         }
//         stage('Deliver') { 
//             steps {
//                 sh './jenkins/scripts/deliver.sh' 
//                 input message: 'Finished using the web site? (Click "Proceed" to continue)' 
//                 sh './jenkins/scripts/kill.sh' 
//             }
//         }
//         stage('OWASP Dependency-Check Vulnerabilities') {
//             steps {
//                 dependencyCheck additionalArguments: ''' 
//                             -o './'
//                             -s './'
//                             -f 'ALL' 
//                             --prettyPrint''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
                
//                 dependencyCheckPublisher pattern: 'dependency-check-report.xml'
//             }
//         }
//     }
// }


// pipeline {
// 	agent any
// 	stages {
// 		stage('Checkout SCM') {
// 			steps {
//                 echo "Cloning repository..."
//                 checkout scm
//                 echo "Cloned."
//             }
// 		}

// 		stage('OWASP Dependency-Check Vulnerabilities') {
// 			steps {
//                 dependencyCheck additionalArguments: ''' 
//                             -o './'
//                             -s './'
//                             -f 'ALL' 
//                             --prettyPrint''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
                
//                 dependencyCheckPublisher pattern: 'dependency-check-report.xml'
//             }
// 		}
// 	}	
// 	post {
// 		success {
// 			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
// 		}
// 	}
// }

pipeline {
    agent any
        stages {
            stage ('Checkout') {
                steps {
                    git branch:'master', url: 'https://github.com/OWASP/Vulnerable-Web-Application.git'
                    }
            }
            stage('SonarQube Analysis') {
                steps {
                    script {
                        def scannerHome = tool 'SonarQube';
                        withSonarQubeEnv('SonarQube') {
                            sh """
                                ${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=OWASP \
                                -Dsonar.sources=. \
                                -Dsonar.host.url=http:172.18.0.3:9000 \
                                -Dsonar.token=sqp_e7930410a30c64a8e0b952d9c2ed2c592df6c662
                            """
                        }
                    }
                }
            }
        }
        // post {
        //     always {
        //         recordIssues enabledForFailure: true, tool: sonarQube()
        //     }
        // }
}
