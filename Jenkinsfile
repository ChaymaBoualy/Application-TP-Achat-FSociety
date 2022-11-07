pipeline {
    agent any
	
    environment {
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
    }

    stages {
        stage("git pull"){
            steps{
              
                git branch: 'stockBack', 
                credentialsId: '91ccda11-c578-4756-b7a8-d912db0bed6d', 
                url: 'https://github.com/ChaymaBoualy/Application-TP-Achat-FSociety.git'
                    
                }
                
            }
      //  stage("MVN CLEAN"){
     //       steps{
              
           //     sh 'mvn clean package'
                             
           //     }
                
         //   }
        stage("MVN TEST"){
            steps{
              
                sh 'mvn clean test'
                             
                }
                
            }
        stage("build"){
            steps{
              
                sh 'mvn install package'
                             
                }
                
            }
	    stage("UNIT TEST") {
            steps {
                echo "TESTING project"
                sh 'mvn test -Dmaven.test.failure.ignore=true '
            }
        }
        stage("SonarQube analysis"){
            steps{
            withSonarQubeEnv('sonarqube-8.9.7') {  
                sh 'mvn sonar:sonar'
                sh 'mvn test -Dmaven.test.failure.ignore=true'
                             
                }
             }
                
           }
       stage('nexus deploy') {
             steps{
                sh'mvn deploy  '
           
          }

       }
	  stage("Building image") {
            steps {
                sh 'docker build -t dhafer01/project-achat .'
            }
        }
        stage('Docker Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW'
			}
		} 
        stage('Push') {

			steps {
				sh 'docker push dhafer01/project-achat'
			}
	}
	stage('deploy docker-compose'){
            steps{
                script{
                    sh 'docker-compose up -d'
                }
            }
       
        }    
	
       
    }
	post {
		always {
			sh 'docker logout'
		}
        
	}
} 
       
        
     
