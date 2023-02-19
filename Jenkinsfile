// node {
//     def serverImage
//     def clientImage
//     docker.withRegistry("https://index.docker.io/v1/", "Docker_Hub" ) {
//       stage('Clone repo') {
//         checkout scm
//       }
//       stage('Build server') {
//         serverImage = docker.build("devtraining/server-app:v1.0.0", "./polling-app-server")
//       }
//       stage('Build client') {
//         clientImage = docker.build("devtraining/client-app:v1.0.0", "./polling-app-client")
//       }
//       stage('Push server image') {
//           serverImage.push("${env.BUILD_NUMBER}")
//           serverImage.push()
//       }
//       stage('Push client image') {
//           clientImage.push("${env.BUILD_NUMBER}")
//           clientImage.push()
//       }
//       stage('Trigger ManifestUpdate') {
//                 echo "triggering k8s-polling-app-deploymentjob"
//                 build job: 'k8s-polling-app-deployment', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
//         }
//     }
// }


node {
    def serverImage
    def clientImage
    docker.withRegistry("https://index.docker.io/v1/", "Docker_Hub_Jenkins" ) {
      stage('Clone repo') {
        checkout scm
      }
        
//       steps{
//           script{
//           withSonarQubeEnv('sonarserver') { 
//           sh "mvn sonar:sonar -Dsonar.java.binaries=target/classes"
//            }
//           timeout(time: 1, unit: 'HOURS') {
//           def qg = waitForQualityGate()
//           if (qg.status != 'OK') {
//                error "Pipeline aborted due to quality gate failure: ${qg.status}"
//           }
//         }
//         }
//         }  
//       }
        
      stage('Build server') {
        serverImage = docker.build("devtraining/polling-app-server:v1.0.0", "./polling-app-server")
      }
//       stage('Test image') {
//         serverImage.inside {
//           sh 'echo "Tests passed"'
//         }
//       }
      stage('Build client') {
        clientImage = docker.build("devtraining/polling-app-client:v1.0.0", "./polling-app-client")
      }
//       stage('Test image') {
//         clientImage.inside {
//           sh 'echo "Tests passed"'
//         }
//       }
     
      stage('Push server image') {
          serverImage.push("${env.BUILD_NUMBER}")
          serverImage.push()
      }
      stage('Push client image') {
          clientImage.push("${env.BUILD_NUMBER}")
          clientImage.push()
      }
      stage('Trigger ManifestUpdate') {
                echo "triggering k8s-polling-app-deploymentjob"
                build job: 'k8s-polling-app-deployment', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
    }
}
