pipeline {
     environment {
       IMAGE_NAME = "student-api"
       IMAGE_TAG = "v1"
       STAGING = "joris-staging"
       PRODUCTION = "joris-production"
       IMAGE_REPO = "joriscommin"
     }
     agent none
     stages {
         stage('Build image') {
             agent any
             steps {
                script {
                 sh 'cd simple_api && docker build -t $IMAGE_NAME:$IMAGE_TAG .'
                 //sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG -f ./simple_api/Dockerfile . '
                }
             }
        }
        stage('Run container based on builded image') {
            agent any
            steps {
               script {
                 sh '''
                    docker rm -f $IMAGE_NAME
                    docker run --name $IMAGE_NAME -d -p 80:5000 $IMAGE_REPO/$IMAGE_NAME:$IMAGE_TAG
                    sleep 5
                 '''
               }
            }
       }
       stage('Test image') {
           agent any
           steps {
              script {
                sh '''
                    curl http://172.17.0.1:80 | grep -q "error"
                '''
              }
           }
      }
   }
}
