pipeline {
  agent any
  tools {
    nodejs 'sonarnode'
  }
  environment {
    PATH = 'C:\\Users\\himan\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin;C:\\Windows\\System32'
    NODEJS_HOME = 'C:\\Program Files\\nodejs'
    SONAR_SCANNER_PATH = 'C:\\Program Files\\sonarqube-10.7.0.96327\\bin\\windows-x86-64\\bin'
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Install Dependencies') {
      steps {
        bat '''
        set PATH = %NODEJS_HOME%;%PATH%
        npm install
        '''
      }
    }
    stage('Lint') {
      steps {
        bat '''
        set PATH = %NODEJS_HOME%;%PATH%
        npm run lint
        '''
      }
    }
    stage('Build') {
      steps {  // Fix: change 'setps' to 'steps'
        bat '''
        set PATH = %NODEJS_HOME%;%PATH%
        npm run build
        '''
      }
    }
    stage('SonarQube Analysis') {
      environment {
        SONAR_TOKEN = credentials('sonarqube-token')
      }
      steps {
        bat '''
        set PATH = %SONAR_SCANNER_PATH%;%PATH%
        where sonar-scanner || echo "SonarQube scanner not found. Please install it."
        sonar-scanner -Dsonar.projectKey=mernstack ^
        -Dsonar.sources=. ^
        -Dsonar.host.url=http://localhost:9000 ^
        -Dsonar.token=sqp_e1933c52ed8cf7402b7401d24a33171a0481d284
        '''
      }
    }
  }

  post {
    success {
      echo 'Pipeline completed successfully'
    }
    failure {
      echo 'Pipeline failed'
    }
    always {
      echo 'This runs regardless of the result.'
    }
  }
}
