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
                    sh "curl -u admin:formation-2021 --upload-file target/*.war 'http://10.10.20.31:8081/repository/depot_test/test${BUILD_NUMBER}.war'"        
                  }
          }


          stage('Téléchargement du binaire') { 
                steps { 
                    sh "wget -P /home/jenkins/tomcat/webapps http://10.10.20.31:8081/repository/depot_test/test${BUILD_NUMBER}.war" 
                    sh "mv /home/jenkins/tomcat/webapps/test${BUILD_NUMBER}.war  /home/jenkins/tomcat/webapps/test.war" 
                } 
          } 

         stage('Test de performance') { 
 
             steps { 
                 sh '/home/jenkins/apache-jmeter-5.4.1/bin/jmeter.sh -n -t /home/jenkins/apache-jmeter-5.4.1/bin/hello_test.jmx -l /home/jenkins/test_report.jtl' 
             }    
         } 
          
         stage ('Validation de l\'application') {  
                steps { 
                    sh "curl -u admin:formation-2021 --upload-file /home/jenkins/tomcat/webapps/test.war 'http://{10.10.20.31}:8081/repository/depot_test/app_fiable${BUILD_NUMBER}.war'" 
                } 
         }   
    }
}
