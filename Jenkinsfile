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
    }
    post {
        success {
            echo 'Chart basariyla dogrulandi ve render edildi'
        }
        failure {
            echo 'Chart hatali'
        }
    }
}