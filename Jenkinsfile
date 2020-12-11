pipeline {
   agent { dockerfile true }
   environment {
       registry = "192.168.1.81:5000/chitrankdixit/sargam-payment"
       dockerImage = "chitrankdixit/sargam-payment"
   }
   stages {
       stage('Build') {
           steps{
            script {
              dockerImage = docker.build registry + ":$BUILD_NUMBER"
            }
          }
       }
       stage('Test') {
        steps {
          script {
            docker.withRegistry( "" ) {
              dockerImage.inside() {
               sh 'python manage.py test'
              }
            }
          }
        }
           
       }
       stage('Publish') {
           steps{
            script {
              docker.withRegistry( "" ) {
                dockerImage.push()
              }
            }
          }
       }
       stage ('Deploy') {
           steps {
            script {
              kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
            }
          }
       }
   }
}
