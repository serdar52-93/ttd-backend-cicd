pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh '''
                  docker build -t serdar52/ttd-backend:jenkins-${BUILD_NUMBER} .
                '''
            }
        }
        
        stage('Release') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhubcredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
  
                sh ''' 
                docker login -u $USERNAME -p $PASSWORD
                docker push serdar52/ttd-backend:jenkins-${BUILD_NUMBER}
                '''
                }
            }
        }

        stage('deploy') {
            steps {
                sh ''' 
                docker stop ttd-backend || true
                docker rm -f ttd-backend || true 
                docker run -p5000:5000 -d --name ttd-backend serdar52/ttd-backend:jenkins-${BUILD_NUMBER}
                '''
                }
            }
        
    }
}