pipeline {
    agent { label 'UBUNTU_NODE1' }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/Madhuri-chinta/openmrs-core.git',
                    branch: 'master'
            }
        }
        stage('package') {
            steps {
                sh 'export PATH=/usr/lib/jvm/java-1.8.0-openjdk-amd64/bin:$PATH',
                sh 'mvn clean package'
            }
        }
        stage('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar',
                                 onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}
