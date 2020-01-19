node{
  def Namespace = "pkapp"
  def ImageName = "rakheebala/mkimage"
  def Creds	= "rb-dockerhub-creds"
  def imageTag = "1.0"
  try{
	stage('Checkout'){
		git 'https://github.com/rakheeGitHub/mk-k8-ci-cd.git'
	}

	stage('RUN Unit Tests'){
		sh "/usr/bin/npm install"
		sh "/usr/bin/npm test"
	}
  
	stage('Docker Build, Push'){
		withDockerRegistry([credentialsId: "${Creds}", url: 'https://hub.docker.com/']) {
			sh "docker build -t ${ImageName}:${imageTag} ."
			sh "docker push ${ImageName}"
		}
	}
  
	stage('Deploy on K8s'){
		script{
			//sh "cd ansible/sayarapp-deploy"
			//sh "pwd"
			sh "kubectl create namespace pkapp"
			sh "sudo -i /usr/local/bin/helm install rb-pkapp  --namespace=pkapp --set image.repository=rakheebala/mkimage --set image.tag=1.0"
			//helm upgrade --wait --recreate-pods --namespace=pkapp --set image.repository=rakheebala/mkimage --set image.tag=1.0 --set namespace=pkapp sayar-pkapp sayarapp"
        }  
	}
  }
  catch (err) {
	currentBuild.result = 'FAILURE'
  }
}
