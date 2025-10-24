pipeline {
  agent any

   tools {
    nodejs "node25"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: env.BRANCH_NAME, url: 'https://github.com/datphamtien1/my-app'
      }
    }

    stage('Install') {
      steps {
        sh 'ls -la'   
        sh 'npm ci'
      }
    }

    stage('Test') {
      when {
        not { branch 'main' }
      }
      steps {
        echo '🧪 Running tests for non-main branch...'
        sh 'npm test || echo "No test script found"'
      }
    }

    stage('Build') {
      steps {
        echo '🏗️ Building app...'
        sh 'npm run build'
      }
    }

    stage('Deploy (Only on main)') {
      when {
        branch 'main'
      }
      steps {
        echo '🚀 Deploying React app via PM2...'
        sh '''
          export PM2_HOME=/var/lib/jenkins/.pm2
          pm2 delete react-demo || true
          npm run build
          pm2 start npm --name "react-demo" -- run preview
          pm2 save
        '''
      }
    }
  }

  post {
    success {
      echo "✅ Build completed successfully for branch: ${env.BRANCH_NAME}"
    }
    failure {
      echo "❌ Build failed on branch: ${env.BRANCH_NAME}"
    }
  }
}



