pipeline {
  
  agent none
  
  stages {
    
    stage('Compilation et tests') {

      agent {
        label 'agent_java'
      }

      stages {
  
        stage('Test unitaire & publication') {
    
          steps {
            sh 'mvn test'
          }
      
          post {
      
            always {        
              junit 'target/surefire-reports/*.xml'
            }
        
          }
      
        }

        stage('Analyse statique') {
      
          steps {
        
            withSonarQubeEnv('SonarQube') {
              sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
            }

          }
      
        }

        stage('Compilation') {
    
          steps {  
            sh 'mvn -B -DskipTests clean package'
          }
      
        }
            
        stage('Publication du binaire') {

          steps {
            sh "curl -u admin:formation-2021 --upload-file target/*.war 'http://84.39.43.46:8081/repository/depot_test/rondoudou${BUILD_NUMBER}.war'"        
          }

        }
    
      }
  
    }
    
     
        
        
        
      }
      
    }
    
    stage('Creation de l\'image') {
      
      agent {
        label 'agent_docker'
      }
      
      stages {
        
       
          
        stage('Compilation de l\'image') {
      
          steps {
            sh 'docker build -t tomcat_app /home/jenkins/docker/tomcat_app'
          }
          
        }
        
        stage('Stockage de l\'image') {
              
          steps {
            sh "docker tag tomcat_app reeban/tomcat_app:${BUILD_NUMBER}"
            sh 'docker tag tomcat_app reeban/tomcat_app'
            sh 'docker login -u reeban -p formation-2021'
            sh "docker push reeban/tomcat_app:${BUILD_NUMBER}"
            sh 'docker push reeban/tomcat_app'
          }
         
        }
      }
    }
  }
}
