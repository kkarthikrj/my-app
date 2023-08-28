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
	 sh 'docker build -t itsmekarthik/my-app:0.0.2 .' 
	 }
       stage('Push Docker Image'){   
	withCredentials([string(credentialsId: '0260cbb6-8b5d-4702-80f1-16038ef86adc', variable: 'dockerpwd')]) {
	sh 'docker login -u itsmekarthik -p ${dockerpwd}'	
           }
	sh 'docker push itsmekarthik/my-app:0.0.2'
	}
	stage('Remove Previous Container'){
	try{
	sh 'docker rm -f tomcattest'
	}catch(error){
	}
	}
      stage('Run Container on Server'){   
	def dockerRun = 'docker run -p 9090:8080 -d --name my-app itsmekarthik/my-app:0.0.2'
	sshagent(['dockercon']) {
	sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.11.58 ${dockerRun}"	
            }
           }
}
