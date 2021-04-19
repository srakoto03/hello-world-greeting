 pipeline {
  
    agent {
      label 'agent_java'
    }
    
    stages {
    
        stage('Test unitaire') {    
        
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
                sh "curl -u admin:formation-2021 --upload-file target/*.war 'http://10.10.20.31:8081/repository/depot_test/rondoudou${BUILD_NUMBER}.war'"        
              }
      }
    }
}
