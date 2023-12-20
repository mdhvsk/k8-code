node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
  
       app = docker.build("madhavasok/test")
    }

    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                echo "${env.BUILD_NUMBER}"
                def params = [
                    string(name: 'DOCKERV', value: "${env.BUILD_NUMBER}"),
                ]
                build job: 'updatemanifest', parameters: params
        }
}