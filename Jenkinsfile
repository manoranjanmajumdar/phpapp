node{
    stage('Scm Checkout'){
     git branch: 'main', credentialsId: 'git_hub', url: 'https://github.com/manoranjanmajumdar/phpapp.git'
    }
    stage('Mvn Package'){
     def mvnHome = tool name: 'maven-3', type: 'maven'
     def mvnCMD = "${mvnHome}/bin/mvn"
     sh "${mvnCMD} clean package"
    }
    stage('Build Docker Image'){
     sh 'docker build -t manoranjanmajumdar/cymune:6.0.0 .'
    }
    stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]) {
     sh "docker login -u manoranjanmajumdar -p ${dockerHubPwd}"
    }
     sh 'docker push manoranjanmajumdar/cymune:6.0.0'
    }
    stage('deploy'){
     sshagent(['dev-server']){
     sh "ssh -o StrictHostKeyChecking=no root@54.147.128.222 'whoami'"
     sh "ssh -o StrictHostKeyChecking=no root@54.147.128.222 'sudo yum update -y'"
     sh "ssh -o StrictHostKeyChecking=no root@54.147.128.222 'ls -l'"
     sh "ssh -o StrictHostKeyChecking=no root@54.147.128.222 'docker pull manoranjanmajumdar/cymune:6.0.0'"
     sh "ssh -o StrictHostKeyChecking=no root@54.147.128.222 'docker run -d -p 8081:80 --name=majumdar manoranjanmajumdar/cymune:6.0.0'"
            
        }
    }
}
