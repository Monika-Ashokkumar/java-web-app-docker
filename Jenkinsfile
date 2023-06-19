node {
    def buildNumber = BUILD_NUMBER
    stage('Checkout') {
        git url: 'https://github.com/Monika-Ashokkumar/java-web-app-docker.git',  branch: 'master'
    }
    stage("Maven Build"){
       def mavenHome= tool name: "Maven_3.9.2", type: "maven" 
       sh "${mavenHome}/bin/mvn clean package"
    }
    stage('Sonar Scan') {
        withSonarQubeEnv('SonarCloud') {
            def mavenHome= tool name: "Maven_3.9.2", type: "maven" 
            sh """
                ${mavenHome}/bin/mvn clean verify sonar:sonar \\
                -Dsonar.host.url=https://sonarcloud.io \\
                -Dsonar.login=13941b4a76363eb74e34f8f7b391f5fad7991d76 \\
                -Dsonar.organization=monika-ashokkumar \\
                -Dsonar.projectKey=Monika-Ashokkumar_java-web-app-docker \\
                -Dsonar.java.binaries=src/main 
                """
        }
    }
    stage("Build Docker Image"){
        sh "docker build -t monikaashokkumar/java-web-app:${buildNumber} ."
    }
    //stage("Login and Push Docker Image") {
    //    withDockerRegistry([credentialsId: "MAK_dockerhub", url:"https://hub.docker.com/repository/docker/monikaashokkumar/java-web-app/general"]) {
    //        docker.push()
    //    }
    //}
}
