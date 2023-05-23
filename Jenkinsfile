pipeline {
    agent any

    stages {
        stage('CheckOut') {
            steps {
                git branch: 'main', url: 'git@github.com:maheswargoud/dev.git'
            }
        }
        stage('DockerLogin') {
            steps {
                sh 'aws ecr get-login-password --region ap-northeast-1 | sudo docker login --username AWS --password-stdin 778168509800.dkr.ecr.ap-northeast-1.amazonaws.com'
            }
        }
        stage('Build') {
            steps {
                sh 'sudo docker build -t 778168509800.dkr.ecr.ap-northeast-1.amazonaws.com/k8s:$BUILD_NUMBER .'
            }
        }
        stage('Push') {
            steps {
                sh 'sudo docker push 778168509800.dkr.ecr.ap-northeast-1.amazonaws.com/k8s:$BUILD_NUMBER'
            }
        }
        stage('Replace build number') {
            steps {
                sh 'sed -i s/number/$BUILD_NUMBER/g k8s/dept.yml'
            }
        }
        stage('Namespace') {
            steps {
                sh 'kubectl apply -f k8s/namespace.yml'
            }
        }
        stage('Deployment') {
            steps {
                sh 'kubectl apply -f k8s/dept.yml'
            }
        }
        stage('Service') {
            steps {
                sh 'kubectl apply -f k8s/svc.yml'
            }
        }
        stage('Ingress') {
            steps {
                sh 'kubectl apply -f k8s/ing.yml'
            }
        }
        stage('Ingress endpoint') {
            steps {
                sh 'kubectl get ingress -n test'
            }
        }
        stage('Ingress pod') {
            steps {
                sh 'kubectl get po -n ingress-nginx'
            }
        }
        stage('Ingress describe') {
            steps {
            sh 'kubectl describe po ingress-nginx-controller-6dc4c69456-7lqwf -n ingress-nginx'
                }
        }
    }
}
