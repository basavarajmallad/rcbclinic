pipeline {
  agent { label 'slave1' }	
    stages {
        stage('Checkout') {             
            steps {
                sh "rm -rf rcbclinic"
                sh "git clone https://github.com/basavarajmallad/rcbclinic.git"
				 sh "cd rcbclinic"
            }
        }
		    stage('Set up Environment') {
        steps {
            sh 'export export JAVA_HOME=$(dirname $(dirname $(readlink -f $(which java))))'            
	    sh 'export MAVEN_HOME=/usr/share/maven'           
        }
    }
           stage('build') {             
            steps {               
                sh "mvn clean package"
                  }
        }
	    	     stage('Run Application') {
            steps {
                echo 'Running Spring Boot application...'
                sh 'nohup mvn spring-boot:run &'
                sleep(time: 15, unit: 'SECONDS') // Wait for the application to fully start

                // Fetch the public IP and display the access URL
                script {
                    def publicIp = sh(script: "curl -s https://checkip.amazonaws.com", returnStdout: true).trim()
                    echo "The application is running and accessible at: http://${publicIp}:8080"
                }
            }
        }
	         stage('Upload Artifact') {
            steps {
                echo 'Uploading artifact...'
                archiveArtifacts artifacts: 'target/petclinic-0.0.1-SNAPSHOT.jar', allowEmptyArchive: true
            }
        }
	          stage('Validate App is Running') {
            steps {
                echo 'Validating that the app is running...'
                script {
                    def response = sh(script: 'curl --write-out "%{http_code}" --silent --output /dev/null http://localhost:8080', returnStdout: true).trim()
                    if (response == "200") {
                        echo 'The app is running successfully!'
                    } else {
                        echo "The app failed to start. HTTP response code: ${response}"
                        currentBuild.result = 'FAILURE'
                        error("The app did not start correctly!")
                    }
                }
            }
        }
        
    }
}
