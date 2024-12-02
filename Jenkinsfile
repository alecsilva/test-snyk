pipeline {
  agent any

stages {
  stage('Snyk Open Source Scan - SCA') {
    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
        snykSecurity additionalArguments: '-debug', failOnIssues: false, failOnError: false, monitorProjectOnBuild: true, 
                                                                projectName: '${JOB_NAME}', severity: 'high', snykInstallation: 'snyk@latest', 
                                                                snykTokenId: 'snyk-api-token'
          }
      }
        
    stage('Snyk Code Scan - SAST') {
        snykSecurity additionalArguments: '--code -debug', failOnIssues: false, failOnError: false, monitorProjectOnBuild: true,
                                                        projectName: '${JOB_NAME}', snykInstallation: 'snyk@latest', 
                                                        snykTokenId: 'snyk-api-token'
    }
}
}
