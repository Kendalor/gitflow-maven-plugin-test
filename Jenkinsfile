pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
	  	stage('SonarQube analysis') {
	  		steps {
			    withSonarQubeEnv(credentialsId: 'testID for SonarQubeServer', installationName: 'SonarQubeServer') { // You can override the credential to be used
			      sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.6.0.1398:sonar'
			    }
		    }
	  	}
        stage('Deliver') { 
            steps {
            	script {
	                //sh './jenkins/scripts/deliver.sh' 
	                projectPOM = readMavenPom file: "pom.xml"
	                echo "Version: ${projectPOM.version}"
	                echo "ArtefactId: ${projectPOM.artifactId}"
	                projectPOM.groupId 
	                }
            }
        }
	}
}