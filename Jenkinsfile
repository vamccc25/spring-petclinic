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

    }
        environment {
           SONAR_HOME = tool name: 'sonar-scan'
        }
        steps {    

           // Install Node.js only if not already installed
            sh '''
            if ! command -v node >/dev/null 2>&1; then
                echo "Node.js not found. Installing Node.js..."
                curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
                apt-get install -y nodejs
            else
                echo "Node.js already installed:"
                node -v
            fi
            ''' 

           withSonarQubeEnv('sonar-qube') {
                sh '''$SONAR_HOME/bin/sonar-scanner \
                       -Dsonar.projectKey=myPETC \
                       -Dsonar.projectName=mypetclinc \
                       -Dsonar.sources=src/ \
                       -Dsonar.java.binaries=target/classes \
                       -Dsonar.exclusions=src/test/java/****/*.java \
                       -Dsonar.analysis.mode=publish \
                       -Dsonar.exclusions=**/*.html \
                       -Dsonar.projectVersion=${BUILD_NUMBER}
                
                '''
            }
       }

    
    
    stage('Quality Gate'){
        steps {
            timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
        }

    }
  }

}