pipeline {
    agent { label 'Jdk17-Maven' }
    triggers { pollSCM('* 23 * * 1-5') }
    stages {
        stage('VCS') {
            steps {
                git url:'https://github.com/practicepurpouse/spring-petclinic.git',
                    branch: 'sprint_release_1'
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

        stage('Stash') {
            steps {
                stash name: 'artifact',
                      includes: '**/target/*.jar'
            }
        }

        stage('Deploy to VM-Unstash') {
            agent { label 'vm-linux' }
            steps {
                unstash name: 'artifact'
                sh 'java -jar **/*.jar'
            }
        }
    }
}