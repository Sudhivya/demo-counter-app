pipeline{
    
    agent any 
    tools { 
      maven 'M2_HOME' 
    }    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/Sudhivya/demo-counter-app.git'
                }
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
             }
         }
         stage('static code analysis'){
               
             steps{
                 script{
                      withSonarQubeEnv(credentialsId: 'sonar-api-key') {
                      sh 'mvn clean package sonar:sonar'
                   }
                 }
             }
          }
          stage('Quality Gate status'){
             
             steps{
                 
                  script{
                      
                     waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api-key'
                  }
              }
          }
        stage('Upload jar file to nexus'){
            
               steps{
                  
                  script{
                      nexusArtifactUploader artifacts:
                      [
                         [
                            artifactId: 'springboot',
                            classifier: '', file: 'target/Uber.jar',
                            type: 'jar'
                            ]
                      ], 
                      credentialsId: 'nexus-auth',
                      groupId: 'com.example', 
                      nexusUrl: '52.66.210.98:8081', 
                      nexusVersion: 'nexus3',
                      protocol: 'http',
                      repository: 'demoapp-release', 
                      version: '1.0.0'
                      }
                  }
              }
     }
}
