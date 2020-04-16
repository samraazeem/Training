pipeline{ environment {
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
                bat 'docker build -t samraazeem/maven:%BUILD_NUMBER% .'
            }
        }
        stage('Deploy image') {
            steps{
                bat 'docker push samraazeem/maven:%BUILD_NUMBER%'
            }
        }   
        stage('Run Latest container'){
            steps{
                bat 'docker rm -f samra'
                bat 'docker run -d --name samra -p 80:8080 samraazeem/maven:%BUILD_NUMBER%'
            }
        }
    }

}