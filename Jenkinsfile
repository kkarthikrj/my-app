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
	withCredentials([string(credentialsId: 'docker', variable: 'dockerpassword')]) {
	sh 'docker login -u itsmekarthik -p ${dockerpassword}'	
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
	def dockerRun = 'docker run -p 9090:8080 -d --name myapp itsmekarthik/myapp123'
	sshagent(['58c66ad9-d4dd-4fbe-bcb5-0ac42fefe6b9']) {
	sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.15.108 ${dockerRun}"	
            }
           }
}
