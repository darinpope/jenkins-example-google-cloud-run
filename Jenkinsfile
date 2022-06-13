pipeline {
  agent any
  environment {
    CLOUDSDK_CORE_PROJECT='insights-api-localdev'
    CLIENT_EMAIL='insights-api-localdev@appspot.gserviceaccount.com'
    GCLOUD_CREDS=credentials('gcloud-creds')
  }
  stages {
    stage('test') {
      steps {
        sh '''
          gcloud version
          gcloud auth activate-service-account --key-file="$GCLOUD_CREDS"
          gcloud run services list
          gcloud run services replace service.yaml --platform='managed' --region='us-central1'
          gcloud run services list
          gcloud run services add-iam-policy-binding hello --region='us-central1' --member='allUsers' --role='roles/run.invoker'
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