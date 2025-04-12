pipeline {
    agent any

    environment {
        IMAGE_NAME = 'adnansharief/websites:carvilla'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/adnan-sharief/my-app.git'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig-jenkins', variable: 'KUBECONFIG_FILE')]) {
                    sh '''
                        export KUBECONFIG=$KUBECONFIG_FILE
                        echo "Deploying image $IMAGE_NAME"

                        # Replace image in YAML if needed
                        sed -i "s|image:.*|image: $IMAGE_NAME|" deployment/deployment.yaml

                        kubectl apply -f deployment/deployment.yaml
                        kubectl apply -f deployment/service.yaml

                        kubectl get svc my-app-service
                    '''
                }
            }
        }
    }
}
