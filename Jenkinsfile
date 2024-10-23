pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=asgbuggywebapp100 -Dsonar.organization=asgbuggywebapp100 -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=a6d70c2c88ea353035338a33bde8777f2c7e3f3f'
			}
        } 
        
	stage('RunSCAAnalysisUsingSnyk') {
        steps {		
			withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
				sh 'mvn snyk:test -fn'
			}
		}
	}

      
	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
      }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://022006208139.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
      }
   }
}
