pipeline{ 
    environment {
    registry = "samraazeem/maven"
    registryCredential = 'docker'
  }  
   agent any 
   tools{
     maven 'MAVEN'
}
    stages {
        stage("Code Checkout") {
            steps {
                git url: 'https://github.com/samraazeem/Training.git'
            }
        }
        stage('Build Stage') {
            steps{
                bat 'mvn install'  
            }
        }
        stage('Testing Stage'){
            steps{
               echo 'Test cases passed successfully!!'
            } 
        }
        stage('Building image') { 
            steps{
                script {
                    dockerImage= docker.build registry + ":$BUILD_NUMBER"
                }   
            }
        }
        stage('Deploy Image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
                    }
                }
            }
        }
    }
}