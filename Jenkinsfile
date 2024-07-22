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
        stage('Static Code Analysis') {
            steps {
                script {
                    // Checkstyle Analysis
                    sh '''
                        /var/jenkins_home/apache-maven-3.6.3/bin/mvn checkstyle:checkstyle
                    '''
                    // PMD Analysis
                    sh '''
                        /var/jenkins_home/apache-maven-3.6.3/bin/mvn pmd:pmd pmd:cpd
                    '''
                    // SpotBugs Analysis
                    sh '''
                        /var/jenkins_home/apache-maven-3.6.3/bin/mvn spotbugs:spotbugs
                    '''
                }
            }
        }
        stage('Report') {
            steps {
                publishHTML([
                    reportName: 'Checkstyle Report', 
                    reportDir: 'target/site', 
                    reportFiles: 'checkstyle.html',
                    keepAll: true, 
                    alwaysLinkToLastBuild: true, 
                    allowMissing: false
                ])
                publishHTML([
                    reportName: 'PMD Report', 
                    reportDir: 'target/site', 
                    reportFiles: 'pmd.html',
                    keepAll: true, 
                    alwaysLinkToLastBuild: true, 
                    allowMissing: false
                ])
                publishHTML([
                    reportName: 'CPD Report', 
                    reportDir: 'target/site', 
                    reportFiles: 'cpd.html',
                    keepAll: true, 
                    alwaysLinkToLastBuild: true, 
                    allowMissing: false
                ])
                publishHTML([
                    reportName: 'SpotBugs Report', 
                    reportDir: 'target/site', 
                    reportFiles: 'spotbugs.html',
                    keepAll: true, 
                    alwaysLinkToLastBuild: true, 
                    allowMissing: false
                ])
            }
        }
    }
    post {
        always {
            junit 'target/surefire-reports/**/*.xml'
            archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true
        }
    }
}
