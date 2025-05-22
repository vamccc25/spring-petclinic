pipeline {
     
  agent any 

  stages { 

    stage("Build"){
        steps {
            sh "./mvnw install"
        }
    }

    stage("Run unit-test"){
        steps {
            sh "./mvnw test"
        }
    }

    stage("CodeScanning"){
        environment {
           SONAR-HOME = tool name: 'sonar-scan'
        }
        steps {
           withsonarQubeEnv('sonar qube') {

                sh "${SONAR_HOME}/bin/sonar-scanner"
           }
        
       }

    }
    
  }

}