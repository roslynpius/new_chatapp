pipeline {
    agent { label 'Jenkins-Slave' }

    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Correctly assigning the tool installation path to a variable
                    def scannerHome = tool name: 'sonarqube', type: 'SonarQubeScanner'
                    // Use the tool with correct environment variable
                    withSonarQubeEnv('sq-server-1') {
                        // Using the scanner with the scannerHome variable
                        sh "${scannerHome}/bin/sonar-scanner"
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
                    sh 'ssh jenkins@10.0.2.58 "sudo -u chatapp /usr/local/bin/script.sh"'
                }
            }
        }
    }
}
