pipeline {
  agent any
  tools { 
        maven 'maven'  
    }
   stages{
          stage('git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/KommaHemanth/devsecopsproject.git'
            }
        }

    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=kommahemanth -Dsonar.organization=kommahemanth -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=5aaa6ed616125359d36932c3adcedcdc813892d9'
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
               
                 sh 'mvn clean install'
               
            }
    }


	   
	stage ('wait_for_testing'){
	   steps {
		   sh 'pwd; sleep 180; echo "Application Has been deployed on the server"'
	   	}
	   }
	   
	stage('RunDASTUsingZAP') {
          steps {
		    dependencyCheck additionalArguments: '--format HTML', odcInstallation: 'Dependency-Check'   
	     }
       } 
  }
}
