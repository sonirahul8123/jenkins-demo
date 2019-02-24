node {
    stage('First : SCM Checkout'){
        git 'https://github.com/sonirahul8123/hello-world.git'
    }
    
    stage('Second : Build Maven package'){
        sh 'mvn clean package'
    }
    
    stage('Third : Build Docker Image'){
        sh 'docker build -t sonirahul8123/hello-world:2.0.0 .'
    }
    
    stage('Four : Push Docker Image to DockerHub'){
        withCredentials([string(credentialsId: 'docker_pass', variable: 'dockerHubPass')]) {
            sh "docker login -u sonirahul8123 -p ${dockerHubPass}"
        }
        sh 'docker push sonirahul8123/hello-world:2.0.0'
    }
    
    stage('Five : Run a container on web server'){
        def dockerRun = 'docker run -p 8080:8080 -d --name hello-world-app sonirahul8123/hello-world:2.0.0'
        sshagent(['web_server']) {
            sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.6.143 ${dockerRun}"
        }
    }
}
