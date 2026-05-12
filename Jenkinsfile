pipeline {

    agent any

    triggers {

        cron('H/5 * * * *')
    }

    environment {

        IMAGE_NAME = "hello-devops:v1"
    }

    stages {

        stage('Git Clone') {

            steps {

                git 'https://github.com/vinithcriz/hello-devops.git'
            }
        }

        stage('Check Files') {

            steps {

                sh '''
                pwd
                ls -la
                '''
            }
        }

        stage('Build Docker Image') {

            steps {

                sh '''

                eval $(minikube docker-env)

                docker build -t $IMAGE_NAME .

                docker images
                '''
            }
        }

        stage('Deploy Kubernetes') {

            steps {

                sh '''

                kubectl apply -f deployment.yaml

                kubectl get pods

                kubectl get svc
                '''
            }
        }

        stage('Verify Deployment') {

            steps {

                sh '''

                kubectl rollout status deployment/hello-devops
                '''
            }
        }
    }

    post {

        success {

            echo 'Application Deployed Successfully'
        }

        failure {

            echo 'Pipeline Failed'
        }
    }
}
