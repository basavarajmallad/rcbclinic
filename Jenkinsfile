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
	           stage('Upload Artifact') {
            steps {
                echo 'Uploading artifact...'
                archiveArtifacts artifacts: 'target/petclinic-0.0.1-SNAPSHOT.jar', allowEmptyArchive: true
            }
        } 
	 	    	     stage('Run Application') {
            steps {
                echo 'Running Spring Boot application...'
                sh 'mvn spring-boot:run '

            }
        }
    }
}
