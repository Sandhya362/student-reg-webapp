node {
    def maven_name = tool name: 'Maven-3.9.10', type: 'maven'
    stage ("git clone") {
        git branch: 'Development', credentialsId: 'GitHubcred', url: ' https://github.com/Sandhya362/student-reg-webapp.git'
        
    }
    
    stage ("maven build") {
        withCredentials([string(credentialsId: 'sonar_token', variable: 'sonar_token')]) {
        sh "${maven_name}/bin/mvn clean verify sonar:sonar -Dsonar.token=${sonar_token}"
        }
    }
      stage ("maven build") {
        sh "${maven_name}/bin/mvn clean deploy"
}
     stage ("stopping the tomcat") {
         sshagent(['tomcat']) {
             sh """
             echo stopping the tomacat server"
             ssh -o StrictHostKeyChecking=no ec2-user@3.108.254.149 sudo systemctl stop tomcat"
             sleep 10 """ }
     }
       stage ("deploy war file to tomcat") {
           sshagent(['tomcat']) {
          sh "scp -o StrictHostKeyChecking=no target/student-reg-webapp.war ec2-user@3.108.254.149:/opt/tomcat/webapps/student-reg-webapp.war"
}
       }
       stage ("start tomcat") {
            sshagent(['tomcat']) {
                sh """
                echo start the tomcat server"
                ssh -o StrictHostKeyChecking=no ec2-user@3.108.254.149 sh /opt/tomcat/bin/startup.sh"
                """
            }      
       }
}
