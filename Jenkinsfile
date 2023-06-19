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
            withCredentials([
                string(credentialsId: 'MAK_SonarLogin', variable: 'sonarLoginKey'),
            ]) {
                 sh """
                ${mavenHome}/bin/mvn clean verify sonar:sonar \\
                -Dsonar.host.url=https://sonarcloud.io \\
                -Dsonar.login=${sonarLoginKey} \\
                -Dsonar.organization=monika-ashokkumar \\
                -Dsonar.projectKey=Monika-Ashokkumar_java-web-app-docker \\
                -Dsonar.java.binaries=src/main 
                """
        }
        }
    }    
    stage("Build Docker Image"){
        sh "docker build -t monikaashokkumar/java-web-app:${buildNumber} ."
    }
}
