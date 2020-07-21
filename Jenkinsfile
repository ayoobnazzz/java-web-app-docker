node{
    stage("git clone"){
        git credentialsId: 'git_cred', url: 'https://github.com/ayoobnazzz/java-web-app-docker.git'
    }
    
    stage("Maven build"){
        
        def mavenHome = tool name:"Maven-3.6.3", type: "maven"
        def mavenCMD = "${mavenHome}/bin/mvn"
        sh "${mavenCMD} clean package"
    }
    
    stage("Build Docker Image"){
      
        sh "docker build -t ayoobnaz/java-web-app ."
    }
    
    stage("Push Docker Image"){
        withCredentials([string(credentialsId: 'docker_cred', variable: 'docker_cred')]) {
            sh "docker login -u ayoobnaz -p ${docker_cred}"
        }
        sh "docker push ayoobnaz/java-web-app"
    }
    
    stage("Deploy Application in K8 Cluster"){
		                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', 
	                        accessKeyVariable: 'AWS_ACCESS_KEY_ID', 
	                        credentialsId: 'AWS_Credentials', 
	                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]){
							    withCredentials([kubeconfigFile(credentialsId: 'EKS-credentials-kubeconfig', variable: 'KUBECONFIG')])
    
								{
								sh 'kubectl apply -f javawebapp-deployment.yml'
								}
								}
								
							}
    
    
}
