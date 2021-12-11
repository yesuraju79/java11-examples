pipeline {
    agent { label 'jdk11-mvn3.8.4' }
    triggers { 
        cron('45 23 * * 1-5')
        pollSCM('*/5 * * * *')
    }
    parameters {
        string(name: 'MAVEN_GOAL', defaultValue: 'package', description: 'This is maven goal' )
        choice(name: 'BRANCH_TO_BUILD', choices: ['declarative', 'master', 'scripted'], description: 'Branch to build')
    }
    stages {
        stage('scm') {
            steps {
                mail from: "devops@qt.com",
                to: 'team@qt.com',
                subject: "Cloning code for project ${env.JOB_NAME} started",
                body: "${env.BUILD_URL}"

                git url: 'https://github.com/GitPracticeRepo/java11-examples.git', branch: "${params.BRANCH_TO_BUILD}"
            }
        }
        stage('build') {
            steps {
                mail from: "devops@qt.com",
                to: 'team@qt.com',
                subject: "Build using maven for project ${env.JOB_NAME} started",
                body: "${env.BUILD_URL}"
                withSonarQubeEnv(installationName: 'SONAR_9.2.1', envOnly: true, credentialsId: 'SONAR_TOKEN') {
                    sh "/usr/local/apache-maven-3.8.4/bin/mvn clean package sonar:sonar"
					echo "${env.SONAR_HOST_URL}"
                    
                }
                
            }
        }
    }
    post {
        always {
            

            emailext attachLog: true,
                body: """<p> Executed: Job <b>\'${env.JOB_NAME}:${env.BUILD_NUMBER}\'
                </b></p><p>View console outpe at "<a href="${env.BUILD_URL}">${env.JOB_NAME}:${env.BUILD_NUMBER}
                </a>"</p> <p><i>Build log is attached </i> </p>""",
                compressLog: true,
                replyTo: "do-not-reply@qt.com",
                to: "qtdevops@gmail.com",
                subject: "${env.JOB_NAME} - Build ${env.BUILD_NUMBER} -Status ${currentBuild.result}"


        }
    }
}
