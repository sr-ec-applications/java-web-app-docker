node{
    def buildNumber = BUILD_NUMBER
    stage("Git Clone"){
        git url: 'https://github.com/sr-ec-applications/java-web-app-docker.git', branch: 'master'
    }
    stage("Maven Clean Package"){
        def mavenHome= tool name: "Maven",type: "maven"
        sh"${mavenHome}/bin/mvn clean package"
    }
    stage("Build Docker Image"){
        sh "docker build -t sekharmoti/java-web-app-docker:${buildNumber} ."
    }
    stage("Docker Login And Push"){
        withCredentials([string(credentialsId: 'Password', variable: 'Password')]) {
            sh "docker login -u sekharmoti -p ${Password}"
            }
        sh "docker push sekharmoti/java-web-app-docker:${buildNumber}"
    }
    stage("Deploy Application As Docker Container In Docker Deployment Server"){
        sshagent(['Password2']) {
            sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.6.252 docker rm -f javawebappcontainer || true"
            sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.6.252 docker run -d -p 8080:8080 --name javawebappcontainer sekharmoti/java-web-app-docker:${buildNumber}"
            }
        
    }
    
}
