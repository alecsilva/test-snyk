pipeline {
    agent any

    stages {
        stage('Snyk Open Source Scan - SCA') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    snykSecurity failOnIssues: false, 
                                 failOnError: false, 
                                 monitorProjectOnBuild: true, 
                                 projectName: '${JOB_NAME}', 
                                 severity: 'high', 
                                 snykInstallation: 'snyk@latest', 
                                 snykTokenId: 'snyk-api-token'
                }
            }
        }

        stage('Snyk Code Scan - SAST') {
            steps {
                snykSecurity additionalArguments: '--code', 
                             failOnIssues: false, 
                             failOnError: false, 
                             monitorProjectOnBuild: true, 
                             projectName: '${JOB_NAME}', 
                             snykInstallation: 'snyk@latest', 
                             snykTokenId: 'snyk-api-token'
            }
        }
    }
}
