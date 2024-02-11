pipeline {
    agent {label 'Jenkins-Slave'}

    tools {
        maven "Maven"
    }

    stages {
	stage('SonarQube Analysis') {
            steps {
                script {
                    def mvn = tool 'Maven';
                    withSonarQubeEnv('sq-server-1') {
                        sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=greetapp_pipeline -f pom.xml"
                    }
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
                             sh 'ssh jenkins@10.0.2.58 "sudo -u greetings /usr/local/bin/deploy_greeting.sh"'
                              }
                        }
                     }
    }
}
