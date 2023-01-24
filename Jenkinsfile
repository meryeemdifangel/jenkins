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
  
        always {
        echo "End of Pipeline process"
        mail(subject: 'End of Process Pipeline : Result incoming ...', body: 'End of Process Pipeline : Result incoming ...', from: 'jm_dif@esi.dz', to: 'jm_dif@esi.dz')
      }
      failure {
        echo "Deployment failed"
        mail(subject: 'Deployment failed', body: 'Deployment failed ', from: 'jm_dif@esi.dz', to: 'jm_dif@esi.dz')
      }
      success {
        echo "Deployment succeeded"
        mail(subject: 'Deployment succeeded', body: 'Deployment succeeded ', from: 'jr_belbachir@esi.dz', to: 'ryan.belbachir01@gmail.com')
        notifyEvents message: 'Hello folks : <b>Deployment succeeded</b> ! ', token: '468PjL-D39IzKutPcnFPLs4vG2bOZWgi'
      }
    }

}
