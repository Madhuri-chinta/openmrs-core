pipeline {
    agent { label 'UBUNTU_NODE1'}
    //triggers { cron ('*/15 * * * 0') } with cron 
    triggers { pollSCM ('*/15 * * * 0') }
    parameters { string(name: 'MVN_GOAL', defaultValue: 'package', description: 'maven package') }
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
                // sh 'export "PATH=/usr/lib/jvm/java-8-openjdk-amd64/bin:$PATH"' (no tool configuration)
                sh "mvn ${params.MVN_GOAL}"
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