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
        
    }
}
