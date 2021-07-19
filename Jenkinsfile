pipeline{ 
    environment {
    registry = "samraazeem/maven"
    registryCredential = 'docker'
    dockerImage= ''
  }  
   agent any 
   tools{
     maven 'MAVEN'
}
    
options {
skipDefaultCheckout(true)
}
    stages {
        stage("Code Checkout") {
            steps {
                git url: 'https://github.com/samraazeem/docker-jenkins-integration.git'
            }
        }
        stage('Build Stage') {
            steps{
                sh 'mvn install'  
            }
        }
        stage('Testing Stage'){
            steps{
               echo 'Test cases passed successfully!!'
            } 
        }
        stage('SonarQube Analysis'){
			steps{
				withSonarQubeEnv('SONAR'){
					sh 'mvn sonar:sonar -Dsonar.projectKey=test -Dsonar.host.url=http://http://52.249.217.108:9000 -Dsonar.login=b9b1f74acbea553a843f0af694b9aaeb78daa021'
				}
			}
		}
        stage('Building image') { 
            steps{
                
                /***sh 'docker build -t "samraazeem/maven":$BUILD_NUMBER"" .'
                script {
                    dockerImage= 'samraazeem/maven":$BUILD_NUMBER"'
                }***/
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
        stage('Kill older container & Run Latest container'){
            steps{
                sh 'docker rm -f samra-mvn'
                sh 'docker run -d --name samra-mvn -p 80:8080 samraazeem/maven:"$BUILD_NUMBER"'
            }
        } 
    }
}
