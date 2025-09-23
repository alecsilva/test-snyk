pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        echo 'Building...'
      }
    }

    stage('Snyk Open Source Scan - SCA (plugin)') {
      steps {
        snykSecurity(
          additionalArguments: '--all-projects --policy-path=.snyk', // sin --debug
          failOnIssues: true,
          failOnError: false,
          severity: 'critical',
          monitorProjectOnBuild: true,
          snykInstallation: 'mi-snyk-arm64',             // Tool Installation manual
          snykTokenId: 'snyk-api-token'     // ID de tu credencial "Snyk API token"
        )
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying...'
      }
    }
  }
}
