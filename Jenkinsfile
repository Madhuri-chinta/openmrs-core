pipeline {
    agent { label 'UBUNTU_NODE1'}
    triggers { cron ('*/15 * * * 0') }
    stages {
        stage ('vcs') {
            steps {
                git url: 'https://github.com/openmrs/openmrs-core.git',
                    branch: 'master'
            }
        }
        stage ('build') {
            tools {
                jdk 'JDK_8'
            }
            steps {
                sh 'mvn package'
            }  
        }    
        stage ('archiveArtifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/openmrs.war',
                                 onlyIfSuccessful: true,
                                 allowEmptyArchive : false
            }
        }
        stage ('testResults') {
            steps {                         
                junit testResults: '**/surefire-reports/TEST-*.xml',
                      allowEmptyResults: true          
            }
        }
        
    }
}