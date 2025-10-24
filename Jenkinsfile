pipeline {
  agent any

  tools {
    nodejs 'node25'
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/datphamtien1/my-app'
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'npm ci'
      }
    }

    stage('Start React App (Preview)') {
      steps {
        echo '🔄 Restarting React preview server...'
        sh '''
          pm2 delete react-demo || true
          pm2 start npm --name "react-demo" -- start
          pm2 save
        '''
      }
    }
  }

  post {
    success {
      echo "✅ React preview server running on port 3000"
    }
    failure {
      echo "❌ Failed to start React preview server"
    }
  }
}
