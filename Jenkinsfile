pipeline {
    agent {label 'Jenkins-Slave'}

	stages {
		stage('SCM') {
		steps{
	    		checkout scm
		}
	 	 }
	  stage('SonarQube Analysis') {
		steps{
	    def scannerHome = tool 'sonarqube';
	    withSonarQubeEnv() {
	      sh "${scannerHome}/bin/sonar-scanner"
	    }
		}
	  }
		stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
        }
		stage('Deploy') {
                 steps {
                       script {
 
                             sh 'rsync -avz $WORKSPACE jenkins@10.0.2.58:/tmp/'
			     sh 'ssh jenkins@10.0.2.58 "sudo -u chatapp /usr/local/bin/script.sh"'
                              }
                        }
                     }
    }
}
