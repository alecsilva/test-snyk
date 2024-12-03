pipeline {
    agent any

    stages {
        stage('Snyk Open Source Scan - SCA') {
            steps {
                sh 'ls'
                sh 'cd todolist'
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    snykSecurity additionalArguments: '', 
                                 failOnIssues: true, 
                                 failOnError: true, 
                                 monitorProjectOnBuild: true, 
                                 projectName: '${JOB_NAME}', 
                                 severity: 'critical', 
                                 snykInstallation: 'snyk@latest', 
                                 snykTokenId: 'snyk-api-token'
                }
            }
        }

        stage('Snyk Code Scan - SAST') {
            steps {
                snykSecurity additionalArguments: '--code', 
                             failOnIssues: false, 
                             failOnError: true, 
                             monitorProjectOnBuild: true, 
                             projectName: '${JOB_NAME}', 
                             snykInstallation: 'snyk@latest', 
                             snykTokenId: 'snyk-api-token'
            }
        }
    }
}
