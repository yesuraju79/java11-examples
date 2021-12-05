pipeline {
    agent { label 'jdk11-mvn3.8.4' }
    triggers { upstream(upstreamProjects: 'starterproject', threshold: hudson.model.Result.SUCCESS) }
    stages {
        stage('scm') {
            steps {
                git 'https://github.com/GitPracticeRepo/java11-examples.git'
            }
        }
        stage('build') {
            steps {
                sh '/usr/local/apache-maven-3.8.4/bin/mvn clean package'
            }
        }
    }
}
