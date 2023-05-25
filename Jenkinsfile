pipeline {
    agent any

    stages {
        stage('CheckOut') {
            steps {
                git branch: 'main', url: 'git@github.com:maheswargoud/k8s.git'
            }
        }
        stage('DockerLogin') {
            steps {
                sh 'aws ecr get-login-password --region ap-northeast-1 | sudo docker login --username AWS --password-stdin 778168509800.dkr.ecr.ap-northeast-1.amazonaws.com'
            }
        }
        stage('Build') {
            steps {
                sh 'sudo docker build -t 778168509800.dkr.ecr.ap-northeast-1.amazonaws.com/dev:$BUILD_NUMBER .'
            }
        }
        stage('Push') {
            steps {
                sh 'sudo docker push 778168509800.dkr.ecr.ap-northeast-1.amazonaws.com/dev:$BUILD_NUMBER'
            }
        }
        stage('Replace build number') {
            steps {
                sh 'sed -i s/number/$BUILD_NUMBER/g k8s/dept.yaml'
            }
        }
        stage('Namespace') {
            steps {
                sh 'kubectl apply -f k8s/namespace.yaml'
            }
        }
        stage('Deployment') {
            steps {
                sh 'kubectl apply -f k8s/dept.yaml'
            }
        }
        stage('Service') {
            steps {
                sh 'kubectl apply -f k8s/svc.yaml'
            }
        }
        stage('Ingress') {
            steps {
                sh 'kubectl apply -f k8s/ing.yaml'
            }
        }
        stage('Ingress endpoint') {
            steps {
                sh 'kubectl get ingress -n dev-ns'
            }
        }
        stage('Ingress pod') {
            steps {
                sh 'kubectl get po -n ingress-nginx'
            }
        }
        // stage('Ingress describe') {
        //     steps {
        //     sh 'kubectl describe po ingress-nginx-controller-6dc4c69456-7lqwf -n ingress-nginx'
        //         }
        // }
    }
}
