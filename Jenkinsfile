pipeline {
    agent any

    environment {
        SCANNER_HOME=tool 'sonar_scanner'
    }

    stages {
        // stage("Git Checkout") {
        //     steps {
        //         git branch: 'main', url: 'https://github.com/spring-projects/spring-petclinic.git'
        //     }
        // }
        
        stage("Sonar_scan"){
            steps{
                withSonarQubeEnv('sonar'){
                    sh ''' $SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectName=Spring_pet_clinc \
                    -Dsonar.projectKey=Spring_pet_clinc \
                    -Dsonar.organization=devops-sk2023 \
                    -Dsonar.java.binaries=target/classes '''
                }
            }
        }
        stage("Quality Gate"){
            steps{
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar' 
                }
            }
        }
        stage("snyk Scan"){
            steps {
                snykSecurity(
                snykInstallation: 'Snyk_tool',
                snykTokenId: 'snyk_api',
                failOnError: 'false',
                failOnIssues: 'false',
                monitorProjectOnBuild: 'true'
                )
            }
        }
        stage("trivy scan"){
            steps{
                sh "trivy fs . > trivyfs.txt"
        }
    }
    stage("Build") {
            steps {
                 sh "mvn install -DskipTests=true"
                 //sh "mvn clean verify"
            }
        }
     stage ("checking pwd"){
        steps{
            sh "pwd"
    }
}
    stage ("Image build"){
        steps{
            sh "docker build -t suresh10214/petapp:${BUILD_NUMBER} ."
    }
    }
    stage("TRIVY"){
        steps{
            sh "trivy image  suresh10214/petapp:${BUILD_NUMBER} --scanners vuln > trivyimage.txt" 
            }
        }
    stage("Docker Push") {
    steps {
        script {
            docker.withRegistry('https://index.docker.io/v1/', 'docker_credentials') {
                sh "docker push suresh10214/petapp:${BUILD_NUMBER}"
            }
        }
    }
} 

}
}
