pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/jolintanql/vulnado.git'
            }
        }
        stage('Build') {
            steps {
                sh '''
                    /var/jenkins_home/apache-maven-3.6.3/bin/mvn --batch-mode -V -U -e clean verify -Dsurefire.useFile=false -Dmaven.test.failure.ignore
                '''
            }
        }
        stage('Analysis') {
            steps {
                sh '''
                    /var/jenkins_home/apache-maven-3.6.3/bin/mvn --batch-mode -V -U -e checkstyle:checkstyle pmd:pmd pmd:cpd findbugs:findbugs
                '''
            }
        }
    }
}

