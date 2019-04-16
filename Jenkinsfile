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
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') { 
            steps {
            	script {
	                //sh './jenkins/scripts/deliver.sh' 
	                projectPOM = readMavenPom file: "pom.xml"
	                projectPOM.version
	                projectPOM.artefactId
	                projectPOM.groupId 
	                }
            }
        }
    }
}