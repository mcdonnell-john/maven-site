pipeline {
    agent {
        label 'svn-websites'
    }
    stages {
        stage('Build') {
            when {
                not { branch 'master' }
            }
            steps {
                withMaven(jdk:'JDK 1.8 (latest)', maven:'Maven 3 (latest)', mavenLocalRepo:'.repository', options: [
                  artifactsPublisher(disabled: true),
                  junitPublisher(disabled: true),
                  findbugsPublisher(disabled: true),
                  openTasksPublisher(disabled: true)
                ]) {
                    sh "mvn -U clean site"
                }
            }
        }
        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                withMaven(jdk:'JDK 1.8 (latest)', maven:'Maven 3 (latest)', mavenLocalRepo:'.repository', options: [
                  artifactsPublisher(disabled: true),
                  junitPublisher(disabled: true),
                  findbugsPublisher(disabled: true),
                  openTasksPublisher(disabled: true)
                ]) {
                    sh "mvn -U clean site-deploy"
                }
            }
        }
    }
    post {
        always {
            jenkinsNotify()
        }
    }
    options {
        buildDiscarder(logRotator(numToKeepStr:'15'))
        timeout(time: 10, unit: 'MINUTES')
        skipStagesAfterUnstable()
        timestamps()
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
}
