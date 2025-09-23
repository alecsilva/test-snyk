pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        echo 'Building...'
      }
    }

    stage('Diag Snyk CLI') {
      steps {
        // que no mate el pipeline si algo del diag falla
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          sh '''
            set -eux
            BASE="/var/jenkins_home/tools/io.snyk.jenkins.tools.SnykInstallation"
            if [ -d "$BASE" ]; then
              echo "Contenido de $BASE:"
              ls -l "$BASE" || true

              echo "Buscando binarios descargados por el plugin..."
              find "$BASE" -maxdepth 2 -type f -name "snyk*" -print -exec file {} \\; | sed 's/^/  /'

              echo "Primeras 3 líneas (por si fuera HTML):"
              for f in $(find "$BASE" -maxdepth 2 -type f -name "snyk*"); do
                echo "--- $f ---"
                head -n 3 "$f" || true
              done
            else
              echo "Directorio $BASE no existe en este agente. Puede que el plugin use otra ruta."
            fi
          '''
        }
      }
    }

    stage('Snyk Open Source Scan - SCA') {
      steps {
        snykSecurity(
          additionalArguments: '--debug --all-projects --policy-path=.snyk',
          failOnIssues: true,
          failOnError: false,
          severity: 'critical',
          monitorProjectOnBuild: true,
          snykInstallation: 'snyk@latest',
          snykTokenId: 'snyk-api-token'
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
