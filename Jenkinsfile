node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/Anandaws1011/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t anand2592/java-web-app'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Docker_Hub_Pwd')]) {
          sh "docker login -u anand2592 -p ${dockerpassword}"
        }
        sh 'docker push anand2592/java-web-app'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app anand2592/java-web-app'
         
         sshagent(['DOCKER_SERVER']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@13.201.43.211 docker stop java-web-app || true'
          sh 'ssh  ubuntu@13.201.43.211 docker rm java-web-app || true'
          sh 'ssh  ubuntu@13.201.43.211 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@13.201.43.211 ${dockerRun}"
       }
       
    }
     
     
}
