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

        stage('Copy Artifact') {
            steps {
                sh "mkdir -p /tmp/${JOB_NAME}/${BUILD_NUMBER} && cp **/target/*.jar /tmp/${JOB_NAME}/${BUILD_NUMBER}"
            }
        }

        post {
            success {
                mail subject: "The ${JOB_NAME} Build Id ${BUILD_NUMBER} is Success",
                     body: "To know more information about Build click on this URL ${BUILD_URL}",
                     to: 'team-all@devops.com, checkteam@testteam.com',
                     from: 'jenkins@jobs.info'
            }

            failure {
                mail subject: "The ${JOB_NAME} Build Id ${BUILD_NUMBER} is Failure",
                     body: "To know more information about Build click on this URL ${BUILD_URL}",
                     to: 'team-all@devops.com, checkteam@testteam.com',
                     from: 'jenkins@jobs.info'
            }
        }
    }
}