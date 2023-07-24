pipeline {
    agent { label 'UBUNTU_NODE1'}
    //triggers { cron ('*/15 * * * 0') } with cron 
    // triggers { pollSCM ('*/15 * * * 0') } with pollSCM
    //parameters { string(name: 'MVN_GOAL', defaultValue: 'package', description: 'maven package') } only one option
    parameters { choice(name: 'MVN_GOAL', choices: ['package', 'install', 'clean package', 'clean test'], description: 'maven package')}
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
    post {
        failure {
           mail subject : "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is failure",
                body : "Use this URL ${BUILD_URL} for more info",
                from : "${GIT_COMMITTER_EMAIL}", // here pass jenkins environmental variable 
                to : "${GIT_AUTHOR_EMAIL}" // here also pass jenkins environmental variable 
        }
        success {
            mail subject : "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is success",
                 body : "Use this URL ${BUILD_URL} for more info",
                 from : 'madhu123@gmail.com',
                 to : 'sweety123@gmail.com'
        }   
}    
}