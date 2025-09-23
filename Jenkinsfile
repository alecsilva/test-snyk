pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        echo 'Building...'
      }
    }

    stage('Diag Agent') {
      steps {
        sh '''#!/bin/sh
set -eu
echo "Kernel: $(uname -a)"
echo "Arch:   $(uname -m)"
if [ -f /etc/os-release ]; then
  echo "OS:"
  cat /etc/os-release
fi
if [ -f /etc/alpine-release ]; then
  echo "Alpine release: $(cat /etc/alpine-release)"
fi
'''
      }
    }

    stage('Snyk Open Source Scan - SCA (standalone, ARM-safe)') {
      environment {
        SNYK_TOKEN = credentials('snyk-api-token') // <-- tu credencial en Jenkins
      }
      steps {
        sh '''#!/bin/sh
set -eu

ARCH="$(uname -m)"       # esperado: aarch64 si es ARM64
IS_ALPINE=0
[ -f /etc/alpine-release ] && IS_ALPINE=1

# Selección de binario según SO/arquitectura
if [ "$ARCH" = "aarch64" ]; then
  if [ "$IS_ALPINE" -eq 1 ]; then
    URL="https://downloads.snyk.io/cli/stable/snyk-alpine-arm64"
  else
    URL="https://downloads.snyk.io/cli/stable/snyk-linux-arm64"
  fi
else
  # fallback x86_64
  if [ "$IS_ALPINE" -eq 1 ]; then
    URL="https://downloads.snyk.io/cli/stable/snyk-alpine"
  else
    URL="https://downloads.snyk.io/cli/stable/snyk-linux"
  fi
fi

echo "Descargando Snyk CLI: $URL"
curl -fsSL "$URL" -o snyk
chmod +x snyk

./snyk --version

# Escaneo: escribimos JSON a archivo para no mezclar --debug con --json en stdout
# IMPORTANTE: por defecto, si encuentra issues >= --severity-threshold, el exit code será != 0
./snyk test \
  --all-projects \
  --severity-threshold=critical \
  --policy-path=.snyk \
  --json-file-output=snyk-report.json \
  --debug

echo "Snyk JSON generado en: $PWD/snyk-report.json"

# (Opcional) subir snapshot al dashboard de Snyk
# ./snyk monitor --all-projects --policy-path=.snyk --debug || true
'''
        archiveArtifacts artifacts: 'snyk-report.json', onlyIfSuccessful: false
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying...'
      }
    }
  }
}
