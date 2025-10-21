pipeline {
  agent any
  tools {
      nodejs "NodeJS_25"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/bilalsh123/Playwright-CI-demo.git'
      }
    }

    stage('Install dependencies') {
      steps {
        sh '''
          npm ci
          npx playwright install --with-deps
        '''
      }
    }

    stage('Run Playwright tests') {
      steps {
        sh '''
          npx playwright test --reporter=junit,line --output=test-results
        '''
      }
      post {
        always {
          junit 'test-results/results.xml'
          archiveArtifacts artifacts: 'test-results/**', fingerprint: true
        }
      }
    }
  }
}
