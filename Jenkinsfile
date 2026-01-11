pipeline {
  agent any

  stages {
    stage('Install Apache2') {
      steps {
        sh '''
          set -eux
          sudo apt-get update
          sudo apt-get install -y apache2
        '''
      }
    }

    stage('Enable & Start Apache2') {
      steps {
        sh '''
          set -eux
          sudo systemctl enable --now apache2
          sudo systemctl status apache2 --no-pager
        '''
      }
    }

    stage('Generate 404 (for logs)') {
      steps {
        sh '''
          curl -I http://localhost/this-page-does-not-exist || true
        '''
      }
    }

    stage('Check logs for 4xx/5xx') {
      steps {
        sh '''
          echo "---- error.log ----"
          sudo tail -n 20 /var/log/apache2/error.log || true

          echo "---- 4xx/5xx in access.log ----"
          sudo grep -E ' 4[0-9]{2} | 5[0-9]{2} ' /var/log/apache2/access.log | tail -n 20 || true
        '''
      }
    }

    stage('Verify') {
      steps {
        sh '''
          curl -I http://localhost | head -n 5
        '''
      }
    }
  }
}
