pipeline {
    agent any
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/rajkumarqt/ProjectsRelated.git'
            }
        }
        stage('Build docker image') {
            steps {
                sh "docker image build -t rajkumar207/jenkinsdec1623:$BUILD_ID ."
            }
        }
        stage('Trivy Scan') {
            steps {
                script {
                    sh "trivy image --format json -o trivy-report.json rajkumar207/jenkinsdec1623:$BUILD_ID"
                }
                publishHTML([reportName: 'Trivy Vulnerability Report', reportDir: '.', reportFiles: 'trivy-report.json', keepAll: true, alwaysLinkToLastBuild: true, allowMissing: false])
            }
        }
        stage('publish docker image') {
            steps {
                sh "docker image push rajkumar207/jenkinsdec1623:$BUILD_ID"
            }
        }
        stage('Ensure kubernetes cluster is up') {
            steps {
                sh "cd deployment/k8s/ && eksctl create cluster --config-file=cluster.yaml"
            }
        }
        stage('deploy to k8s') {
            steps {
                sh "kubectl apply -f deployment/k8s/deployment.yaml"
                sh """
                kubectl patch deployment netflix-app -p '{"spec":{"template":{"spec":{"containers":[{"name":"netflix-app","image":"shaikkhajaibrahim/jenkinsdec23workshop:$BUILD_ID"}]}}}}'
                """
            }
        }
    }
}
