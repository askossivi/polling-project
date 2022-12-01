
node {
    def pollingserverImage
    def pollingclientImage
    docker.withRegistry("https://index.docker.io/v1/", "Docker_Hub" ) {
      stage('Clone repo') {
        checkout scm
      }

    stage('Clone repository') {
      

        checkout scm
    }

// BUILD AND TEST SERVER  APP
    stage('Build image') {
  
       pollingserverImage = docker.build("devtraining/polling-app-server", "./polling-app-server")
    }

    stage('Test image') {
  

        pollingserverImage.inside {
            sh 'echo "Tests passed"'
        }
    }

// BUIL AND TEST CLIENT APP
    stage('Build image') {
  
       pollingclientImage = docker.build"devtraining/polling-app-client", "./polling-app-client")
    }

    stage('Test image') {
  

        pollingclientImage.inside {
            sh 'echo "Tests passed"'
        }
    }

// PUSH BUILT IMAGES
    stage('Push server image') {
        pollingserverImage.push("${env.BUILD_NUMBER}")
    }
    
    
    stage('Push client image') {
        pollingclientImage.push("${env.BUILD_NUMBER}")
    }


    stage('Trigger ManifestUpdate') {
                echo "triggering k8s-polling-app-deploymentjob"
                build job: 'k8s-polling-app-deployment', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}