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
//     agent any
    
//     stages {
//         stage('Checkout SCM') {
//             steps {
//                 echo "Cloning repository..."
//                 checkout scm
//                 echo "Cloned."
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
//             }
        

//         // stage('Build Server Docker Image') { 
//         //     steps {
//         //         dir('server') {
//         //             script {
//         //                 dockerImage = docker.build("myapp-server", "./server")
//         //             }
//         //         }
//         //     }
//         // }
//     }
//     post {
// 		success {
// 			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
// 		}
// 	}
// }

pipeline {
	agent any
	stages {
		stage('Checkout SCM') {
			steps {
                echo "Cloning repository..."
                checkout scm
                echo "Cloned."
            }
		}

		stage('OWASP Dependency-Check Vulnerabilities') {
			steps {
				dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'Default'
			}
		}
	}	
	post {
		success {
			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		}
	}
}