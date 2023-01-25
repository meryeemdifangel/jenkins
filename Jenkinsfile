pipeline {
  agent any 
  stages {
    stage('Test') 
    {  
    steps{ 
    bat 'gradlew test'  
   archiveArtifacts 'build/test-results/'
                cucumber reportTitle: 'Cucumber report', 
                fileIncludePattern: 'target/report.json',
                trendsLimit: 10,
                classifications: [ 
                    [  
                       'key': 'Browser',
                        'value': 'Firefox'
                    ]
                ]
                      junit 'build/test-results/test/TEST-Matrix.xml'      
    }
    }
    
       stage ('Code Analysis') { // la phase build
            steps {
                           withSonarQubeEnv('jenkins'){
                bat 'gradle sonarqube'
                            }
            }
         }
    
         stage("Quality gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true}
            }
        }
    
      
                 stage("Build") {
            steps {
                bat 'gradle build'
                bat 'gradle javadoc'
                archiveArtifacts 'build/libs/*.jar'
                archiveArtifacts 'build/docs/'
            }
        }

         stage("deploy") {
            steps {
                bat 'gradle publish'

            }
        } 
  }
   
  post {
        always {
        echo "End of Pipeline process"
        mail(subject: 'End of Process Pipeline : Result incoming ...', body: 'End of Process Pipeline : Result incoming ...', from: 'jm_dif@esi.dz', to: 'marissadjeff@gmail.com')
      }
      failure {
        echo "Deployment failed"
        mail(subject: 'Deployment failed', body: 'Deployment failed ', from: 'jm_dif@esi.dz', to: 'marissadjeff@gmail.com')
      }
      success {
        echo "Deployment succeeded"
        mail(subject: 'Deployment succeeded', body: 'Deployment succeeded ', from: 'jm_dif@esi.dz', to: 'marissadjeff@gmail.com')
        notifyEvents message: 'Hello folks : <b>Deployment succeeded</b> ! ', token: '468PjL-D39IzKutPcnFPLs4vG2bOZWgi'
      }
    }

}
