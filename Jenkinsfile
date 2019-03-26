pipeline {
  agent any
  stages {
    stage('prepare Backup') {
      steps {
        sh 'echo "download backup"'
        sh 'mkdir /Data/jenkins/bck'
        sh 'ZIP=$(echo `date +%Y-%b-%d`--0100.zip); mv /Data/*zip /Data/jenkins/bck'
        sh 'cd /Data/jenkins/bck; unzip /Data/jenkins/bck/*zip'
        sh 'cd /Data/jenkins/bck/var/atlassian/application-data/jira/data; tar cvf /Data/jenkins/bck/attachments.tar ./attachments; gzip /Data/jenkins/bck/attachments.tar  '
        sh 'cd /Data/jenkins/bck/var/atlassian/application-data/jira/data; tar cvf /Data/jenkins/bck/avatars.tar ./avatars; gzip /Data/jenkins/bck/avatars.tar  '
        sh 'rm -rf /Data/jenkins/bck/var; rm /Data/jenkins/bck/*xml'
      }
    }
    stage('create PostgreSQL') {
      steps {
        sh '/Data/jenkins/kubectl create -f /Data/jenkins/postgres-claim.yaml'
        sh '/Data/jenkins/kubectl create -f /Data/jenkins/postgres-pod.yaml'
        sh '''while [ `/Data/jenkins/kubectl get po | grep postgres | grep Running | wc -l` -eq 0 ]
do
echo "- Still creating... -"
sleep 30
done'''
        sh '/Data/jenkins/kubectl create -f /Data/jenkins/postgres-service.yaml'
      }
    }
    stage('import DB') {
      steps {
        sh 'cat /Data/jenkins/sql/dbexport.pgsql | /Data/jenkins/kubectl exec -i postgres -- psql -U postgres -d jiradb'
        sh '/Data/jenkins/kubectl cp /Data/jenkins/sql/title.sql postgres:/tmp;/Data/jenkins/kubectl exec -it postgres -- bash -c "psql -U postgres -d jiradb < /tmp/title.sql"  '
      }
    }
  }
}