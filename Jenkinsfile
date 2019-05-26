pipeline {
  agent any
  stages{
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
      post {
        success {
          echo 'Now Archiving...'
          archiveArtifacts artifacts: '**/target/*.war'
        }
      }
    }
    stage('Deploy to staging') {
      steps {
        build job: 'deploy-to-staging'
      }
    }
    state('Deploy to prod') {
      steps {
        timeout(time: 5, unit: 'DAYS') {
          input message:'Approve PRODUCTION deployment?'
        }
        build job: 'deploy-prod'
      }
      post {
        success {
          echo 'Code deployed to production'
        }
        failure {
          echo 'Deployment failed...'
        }
      }
    }
  }
}
