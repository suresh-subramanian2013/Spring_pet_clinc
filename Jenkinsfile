pipeline {
    agent any

    environment {
        SCANNER_HOME=tool 'sonar_scanner'
    }

    stages {
        stage("Git Checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }
        stage("Build") {
            steps {
                // sh "mvn install -DskipTests=true"
            }
        }
        stage("Sonar_scan"){
            steps{
                withSonarQubeEnv('sonar'){
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=devsecops28 \
                    -Dsonar.projectKey=devsecops28_petclinc  -Dsonar.organization=devsecops28 '''
                }
            }
        }
    }
}
