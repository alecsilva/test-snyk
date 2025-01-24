pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        echo 'Building...'
      }
    }
    stage('Snyk Open Source Scan - SCA') {
      steps {
        snykSecurity additionalArguments: '-debug', 
                     failOnIssues: true, 
                     failOnError: true, 
                     monitorProjectOnBuild: true, 
                     snykInstallation: 'snyk@latest', 
                     snykTokenId: 'snykTokenId'
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deploying...'
      }
    }
  }
}
