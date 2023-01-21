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
                           withSonarQubeEnv('sonar'){
                bat 'gradle sonarqube'
                            }
            }
         }
  
    
      
  }

}
