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
	                echo "Version: ${projectPOM.version}"
	                echo "ArtefactId: ${projectPOM.artifactId}"
	                projectPOM.groupId 
	                }
            }
        }
        stage('SSH transfer') {
			 script {
				sshPublisher(
					continueOnError: false, failOnError: true,
					publishers: [
					sshPublisherDesc(
						configName: "${env.SSH_CONFIG_NAME}",
						verbose: true,
						transfers: [
							sshTransfer(
								sourceFiles: "${path_to_file}/${file_name}, ${path_to_file}/${file_name}",
								removePrefix: "${path_to_file}",
								remoteDirectory: "${remote_dir_path}",
								execCommand: "run commands after copy?"
							)
						]
					)
			   		]
			   		)
			 }
			}
    }
}