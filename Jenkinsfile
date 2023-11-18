pipeline {
    agent { label 'Jdk17-Maven' }
    triggers { pollSCM('* * * * *') }
    stages {
        stage('VCS') {
            steps {
                git url:'https://github.com/practicepurpouse/spring-petclinic.git',
                    branch: 'develop'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Post Build') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar',
                                 onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml',
                      allowEmptyResults: false
            }
        }
    }
}