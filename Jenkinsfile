pipeline{
    agent any
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}
  stages{
    stage('Build') {
      steps {
	sh 'rm -rf *.var'
        sh 'jar -cvf swe645-hw2.war -C "webapp" .'     
        sh 'docker build -t rithvikathota/swe645-hw2:latest .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW |docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
       }
    }
    stage("Push image to docker hub"){
      steps {
        sh 'docker push rithvikathota/swe645-hw2:latest'
      }
    }
        stage("deploying on k8")
	{
		steps{
			sh 'kubectl set image deployment/deploy1 container-0=rithvikathota/swe645-hw2:latest -n default'
			sh 'kubectl rollout restart deploy deploy1 -n default'
		}
	} 
  }
 
  post {
	  always {
			sh 'docker logout'
		}
	}    
}
