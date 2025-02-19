@Library('java_demo_pipeline@main') _

pipeline {
  agent { label 'slave2' }	
	environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'
        MAVEN_HOME = '/usr/share/maven'
        PATH = "${JAVA_HOME}/bin:${MAVEN_HOME}/bin:${env.PATH}"
    }
    stages {
        stage('Checkout') {             
            steps {
             //  sh "rm -rf rcbclinic"
              // sh "git clone https://github.com/basavarajmallad/rcbclinic.git"
		// sh "cd rcbclinic"
		checkoutcode()		 
            }
        }
	  
        stage('setupjava17') {             
            steps {
		   //sh "whoami"
		      //echo " installing java 17"
               //sh "sudo apt update"
     		//sh "sudo apt install -y openjdk-17-jdk"
		setupjava('openjdk-17-jdk')
		
		 
            }
        }

	 stage('setupmaven') {             
            steps {  
		 //   echo " installing maveen"
     		//sh "sudo apt install -y maven"
		    setupjava('maven')
            }
        }
           stage('build') {             
            steps {               
               // sh "mvn clean package"
		    buildproject()
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
               // sh 'mvn spring-boot:run '
		    sh 'mvn spring-boot:run -Dspring-boot.run.arguments="--server.port=8084"'

            }
        }
    }
}
