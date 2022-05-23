pipeline {
  agent any
  environment {
    CLOUDSDK_CORE_PROJECT='gleaming-tube-346117'
    CLIENT_EMAIL='jenkins-gcloud@gleaming-tube-346117.iam.gserviceaccount.com'
    GCLOUD_CREDS=credentials('gcloud-creds')
  }
  stages {
    stage('test') {
      steps {
        sh '''
          gcloud version
          gcloud auth activate-service-account --key-file="$GCLOUD_CREDS"
          gcloud compute zones list
        '''
      }
    }
    stage ('ssh'){
        steps{
            sh'''
           gcloud compute ssh wordpress-3-vm --zone=us-central1-c --command="echo y | sudo -S apt install git"
           gcloud compute ssh wordpress-3-vm --zone=us-central1-c --command="sudo rm -r /var/www/html"
           gcloud compute ssh wordpress-3-vm --zone=us-central1-c --command="sudo git clone https://github.com/gidra39/gcp /var/www/html/"
           gcloud compute ssh wordpress-3-vm --zone=us-central1-c --command="sudo chmod -R 777 /var/www"
           gcloud compute ssh wordpress-3-vm --zone=us-central1-c --command="sudo unzip /var/www/html/20180706_novinano_nk_71b6e5d0e46a01132850180706065954_archive.zip -d /var/www/html/"
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
