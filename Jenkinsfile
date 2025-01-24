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
        snykSecurity additionalArguments: '-debug --all-projects --policy-path=.snyk', 
                     failOnIssues: true, 
                     failOnError: false, 
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
