pipeline {
    agent any
    stages {
        stage('Lint') {
            steps {
                sh 'helm lint wordpress-chart'
            }
        }
        stage('Template Render') {
            steps {
                sh 'helm template wordpress-chart > rendered-output.yaml'
                sh 'cat rendered-output.yaml'
            }
        }
        stage('Dry Run') {
            steps {
                sh 'KUBECONFIG=/dev/null helm install wordpress-test wordpress-chart --dry-run=client --debug'
            }
        }
    }
    post {
        success {
            echo 'Tum kontroller basarili, chart deploy edilmeye hazir'
        }
        failure {
            echo 'Chart hatali, deploy edilemez'
        }
    }
}