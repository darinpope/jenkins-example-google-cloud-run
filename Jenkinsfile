pipeline {
  agent any
  environment {
    CLOUDSDK_CORE_PROJECT='insights-api-localdev'
    CLIENT_EMAIL='jenkins@insights-api-localdev.iam.gserviceaccount.com'
    GCLOUD_CREDS=credentials('gcloud-creds')
  }
  stages {
    stage('Verify version') {
      steps {
        sh '''
          gcloud version
        '''
      }
    }
    stage('Authenticate') {
      steps {
        sh '''
          gcloud auth activate-service-account --key-file="$GCLOUD_CREDS"
        '''
      }
    }
    stage('Install service') {
      steps {
        sh '''
          gcloud run services replace service.yaml --platform='managed' --region='us-central1'
        '''
      }
    }
  }
  post {
    always {
      sh 'gcloud auth revoke $CLIENT_EMAIL'
    }
  }
}