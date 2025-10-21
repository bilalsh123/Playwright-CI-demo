pipeline {
    agent any
    tools {
        nodejs "NodeJS_25"
    }

    environment {
        NPM_CACHE = "${WORKSPACE}/.npm"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'mkdir -p $NPM_CACHE'
                sh 'npm ci --cache $NPM_CACHE'
                sh 'npx playwright install --with-deps'
            }
        }

        stage('Run Playwright tests') {
            steps {
                sh 'npx playwright test --reporter=html'
            }
            post {
                always {
                    archiveArtifacts artifacts: 'playwright-report/**', fingerprint: true
                    publishHTML(target: [
                        allowMissing: true,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'playwright-report',
                        reportFiles: 'index.html',
                        reportName: 'Playwright Test Report'
                    ])
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'playwright-report/**', fingerprint: true
        }
    }
}
