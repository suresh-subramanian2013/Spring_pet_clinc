pipeline {
    agent any

    stages {
        stage("Git Checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }
        stage("Build") {
            steps {
                sh "mvn clean package"
            }
        }
    }
}
