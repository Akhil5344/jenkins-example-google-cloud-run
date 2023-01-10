pipeline {
  agent any
  environment {
    CLOUDSDK_CORE_PROJECT='round-axiom-372206'
    CLIENT_EMAIL='411912745665-compute@developer.gserviceaccount.com'
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
    stage('Allow allUsers') {
      steps {
        sh '''
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
