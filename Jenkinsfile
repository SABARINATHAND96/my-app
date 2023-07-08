
node{
   stage('SCM Checkout'){
     git 'https://github.com/SABARINATHAND96/my-app.git'
   }
   stage('maven-buildstage'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
    stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Image'){
	sh 'docker build -t sabarinathand96/myweb:0.0.2 .'
	}

	stage('Docker Image Push'){
	withCredentials([string (credentialsId: 'dockerPass' , variable :'dockerPassword')]) {
	sh "docker login -u sabarinathand96 -p ${dockerPassword}"
	}
	sh "docker push sabarinathand96/myweb:0.0.2"
	}
	stage('Remove Previous Container'){
		try{
			sh 'docker rm -rf tomcattester'
		}catch(error){
			//do nothing if there is an exception
		}
	}
	stage('Docker Deployment'){
	sh 'docker run -d -p 8091:8080 --name tomcattester sabarinathand96/myweb:0.0.2'
	}
	
}
