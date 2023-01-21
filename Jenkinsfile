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
                
      post {
        always {
            junit 'build/test-results/test/TEST-Matrix.xml'
        }
    }
    }
    }
  }

}
