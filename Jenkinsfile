pipeline {
  agent any
  stages {
    stage('prepare Backup') {
      steps {
        sh 'echo "backup letöltése"'
        sh 'mkdir /Data/jenkins/bck'
        sh 'ZIP=$(echo `date +%Y-%b-%d`--0100.zip); mv /Data/$ZIP /Data/jenkins/bck'
        sh 'cd /Data/jenkins/bck; unzip /Data/jenkins/bck/*zip'
        sh '/Data/jenkins/bck/var/atlassian/application-data/jira/data; tar cvf /Data/jenkins/bck/attachments.tar ./attachments; gzip /Data/jenkins/bck/attachments.tar  '
      }
    }
  }
}