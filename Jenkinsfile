pipeline{
    agent any 
}

stages{
    stage("Git Checkout"){
        steps{
            git branch: 'master', url: 'https://github.com/spring-projects/spring-petclinic.git'
        }
        
    }
    stage("Build"){
        steps{
            sh "mvn clean package"
        }
}
}