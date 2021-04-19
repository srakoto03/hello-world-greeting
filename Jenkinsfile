pipeline {   

    agent {
        label 'agent_java'
      }
    stages {
     stage('Test unitaire') {
              steps {
                sh 'mvn test'
              }    
              post { 
                always {        
                    junit 'target/surefire-reports/*.xml'
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
                sh "curl -u admin:formation-2021 --upload-file target/*.war 'http://10.10.20.31:8081/repository/depot_test/test-maven${BUILD_NUMBER}.war'"        
              }
      }


      stage('Téléchargement du binaire') { 
            steps { 
                sh "wget -P /home/jenkins/tomcat/webapps http://10.10.20.31:8081/repository/depot_test/test-maven${BUILD_NUMBER}.war" 
                sh "mv /home/jenkins/tomcat/webapps/app${BUILD_NUMBER}.war  /home/jenkins/tomcat/webapps/test-maven.war" 
            } 
      } 

      stage('test de performance') { 
            steps { 
                 sh '/home/jenkins/apache-jmeter/bin/jmeter.sh -n -t ./test.jmx -l /home/jenkins/test_report.jtl' 
            }   
      }
 

    }
 
}
