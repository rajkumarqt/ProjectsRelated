pipeline {
    agent none
    stages {
        stage('clean workspace'){
            agent { label 'docker' }
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            agent { label 'docker' }
            steps{
                git branch: 'main', url: 'https://github.com/rajkumarqt/ProjectsRelated.git'
            }
        }
        stage('Build docker image') {
            agent { label 'docker' }
            steps {
                sh "docker image build -t rajkumar207/jenkinsdec1623:$BUILD_ID ."
            }
        }
        stage('Trivy Scan') {
            agent { label 'docker' }
            steps {
                script {
                    sh "trivy image --format json -o trivy-report.json shaikkhajaibrahim/jenkinsdec23workshop:$BUILD_ID"
                }
                publishHTML([reportName: 'Trivy Vulnerability Report', reportDir: '.', reportFiles: 'trivy-report.json', keepAll: true, alwaysLinkToLastBuild: true, allowMissing: false])
            }
        }
        stage('publish docker image') {
            agent { label 'docker' }
            steps {
                sh "docker image push rajkumar207/jenkinsdec1623:$BUILD_ID"
            }
        }
    }
}