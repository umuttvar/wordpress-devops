pipeline{
    agent any
    stages{
        stage("Helm chart Kontrolu"){
            steps{
                sh 'helm lint wordpress-chart'
            }
        }
    }
    post{
       
        success{
            echo "Helm chart basariyla kontrol edildi"
        }
        failure{
            echo "Helm chart kontrol edilemedi, loglar kontrol edilmeli!"
        }
    }
}