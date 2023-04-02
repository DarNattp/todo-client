node {
    def app
    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
       app = docker.build("darnattp/basic-k8s-todo-client")
    }

    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'darnattp-dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
                echo "trigger updatemanifestjob"
                build job: 'frontend-updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}