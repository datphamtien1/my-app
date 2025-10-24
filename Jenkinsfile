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
         export PM2_HOME=/var/lib/jenkins/.pm2
          pm2 delete react-demo || true
          npm run build
          pm2 start npm --name "react-demo" -- run preview
          pm2 save
           pm2 startup systemd -u jenkins --hp /var/lib/jenkins
        '''
      }
    }
  }

  post {
    success {
      echo "✅ React prev iew server running on port 3000"
    }
    failure {
      echo "❌ Failed to start React preview server"
    }
  }
}
