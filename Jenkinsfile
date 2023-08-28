node{
   stage('SCM Checkout'){
     git 'https://github.com/kkarthikrj/my-app.git'
      }
   stage('maven-buildstage'){
      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
        }
        stage('Build Docker Image'){           
	 sh 'docker build -t itsmekarthik/myapp123 .' 
	 }
       stage('Push Docker Image'){   
	withCredentials([string(credentialsId: 'dockerhub', variable: 'docker')]) {
	sh 'docker login -u itsmekarthik -p ${docker}'	
           }
	sh 'docker push itsmekarthik/myapp123'
	}
	stage('Remove Previous Container'){
	try{
	sh 'docker rm -f tomcattest'
	}catch(error){
	}
	}
      stage('Run Container on Server'){   
	def dockerRun = 'docker run -p 9090:8080 -d --name my-app itsmekarthik/myapp123'
	sshagent(['dockercon']) {
	sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.11.58 ${dockerRun}"	
            }
           }
}
