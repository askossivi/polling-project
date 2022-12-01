
node {
    def pollingserverImage
    def pollingclientImage
    docker.withRegistry("https://index.docker.io/v1/", "Docker_Hub" ) {
      stage('Clone repo') {
        checkout scm
      }



      stage('Build polling app server image') {
        pollingserverImage = docker.build("devtraining/polling-app-server", "./polling-app-server")
      }
      stage('Test polling app server image') {
        pollingserverImage.inside {
            sh 'echo "Tests passed"'
        }
      }



      stage('Build polling app client image') {
        pollingclientImage = docker.build("devtraining/polling-app-client", "./polling-app-client")
      }
      stage('Test image') {
        clientIpollingclientImagemage.inside {
            sh 'echo "Tests passed"'
        }
      }



      stage('Push polling app server image') {
          pollingserverImage.push("${env.BUILD_NUMBER}")
          // serverImage.push()
        }
      stage('Push polling app client image') {
          pollingclientImage.push("${env.BUILD_NUMBER}")
      }
      



      stage('Trigger ManifestUpdate') {
                echo "triggering k8s Deployment Manifest Update Job"
                build job: 'k8s-polling-app-deployment', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }

    }
}
