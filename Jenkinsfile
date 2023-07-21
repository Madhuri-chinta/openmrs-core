pipeline {
    agent { label 'UBUNTU_NODE1'}
    stages {
        stage ('vcs') {
            steps {
                git url: 'https://github.com/openmrs/openmrs-core.git',
                    branch: 'master'
            }
        }
        stage ('build') {
            steps {
                sh 'export "PATH=/usr/lib/jvm/java-8-openjdk-amd64/bin:$PATH" && mvn package'
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