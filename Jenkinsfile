pipeline {
  agent {
    node {
      label 'swe645-hw2'
    }
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh 'rm -rf *.war'
        sh 'jar -cvf swe645-hw2.war -C webapp/ .'
      }
      post {
        success {
          archiveArtifacts artifacts: '*.war', fingerprint: true
        }
      }
    }
    stage('Docker Build and Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh "echo ${PASSWORD} | sudo docker login -u ${USERNAME} --password-stdin"
          sh "sudo docker build -t 'rithvikathota/swe645-hw2' ."
          sh "sudo docker push rithvikathota/swe645-hw2"
        }
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        sh "kubectl rollout restart deploy deploy1 -n default"
      }
    }
  }
}
