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
                     failOnError: false, 
                     monitorProjectOnBuild: true, 
                     severity: 'critical', 
                     snykInstallation: 'snyk@latest', 
                     snykTokenId: 'snykTokenId'
      }
    }
    stage('Snyk Code Scan - SAST') {
      steps {
        snykSecurity additionalArguments: '--code -debug', 
                     failOnIssues: false, 
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
