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
	 sh 'docker build -t itsmekarthik/my-app:0.0.3 .' 
	 }
        }
       stage('Push Docker Image'){   
	withCredentials([string(credentialsId: 'newdockerpwd', variable: 'dockerpassnew')]) {
	sh "docker login -u itsmekarthik -p ${dockerpassnew}"	
           }
	sh 'docker push itsmekarthik/my-app:0.0.3'
	}
      Generate pipeline script
      stage('Run Container on Server'){   
	def dockerRun = 'docker run -p 9090:8080 -d --name my-app itsmekarthik/my-app:0.0.3'
	sshagent(['dockercon']) {
	sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.11.58 ${dockerRun}"	
            }
           }
