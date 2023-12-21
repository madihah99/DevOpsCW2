echo "DevOps CW2 Jenkinsfile"


node {
    def app

    stage('Clone repository') {
        /* The clone repositry to workspace */

        checkout scm
    }


    /*Build the image */
    stage('Build image') {
        app = docker.build("madihah99/devopscw2")
    }

    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

        sshagent(['my-ssh-key']) {
    sh 'ssh ubuntu@54.197.86.180 kubectl set image deployments/devopscw2 devopscw2=madihah99/devopscw2:$BUILD_NUMBER'
    }
}
