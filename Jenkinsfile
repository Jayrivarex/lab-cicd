pipeline {
    agent any
    
    environment {
        AWS_REGION     = 'us-east-1'
        AWS_ACCOUNT_ID = '695454131301'
        ECR_REGISTRY   = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
        IMAGE_REPO     = 'jayrivarex-devops-lab-app'
        IMAGE_TAG      = "${env.BUILD_ID}"
    }

    stages {
        stage('1. Checkout') {
            steps {
                checkout scm
            }
        }

        stage('2. Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_REPO}:${IMAGE_TAG} ."
            }
        }

        stage('3. AWS ECR Login & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'aws-credentials', 
                    usernameVariable: 'AWS_ACCESS_KEY_ID', 
                    passwordVariable: 'AWS_SECRET_ACCESS_KEY'
                )]) {
                    sh """
                        ECR_TOKEN=\$(docker run --rm -e AWS_ACCESS_KEY_ID=\$AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY=\$AWS_SECRET_ACCESS_KEY amazon/aws-cli ecr get-login-password --region ${AWS_REGION})
                        echo \$ECR_TOKEN | docker login --username AWS --password-stdin ${ECR_REGISTRY}
                        docker tag ${IMAGE_REPO}:${IMAGE_TAG} ${ECR_REGISTRY}/${IMAGE_REPO}:${IMAGE_TAG}
                        docker tag ${IMAGE_REPO}:${IMAGE_TAG} ${ECR_REGISTRY}/${IMAGE_REPO}:jenkins
                        docker push ${ECR_REGISTRY}/${IMAGE_REPO}:${IMAGE_TAG}
                        docker push ${ECR_REGISTRY}/${IMAGE_REPO}:jenkins
                    """
                }
            }
        }

        stage('4. Deploy to K8s with Helm') {
            steps {
                sh """
                    helm upgrade --install lab-app ./chart \
                      --set image.tag=${IMAGE_TAG} \
                      -n dev
                """
            }
        }
    }
    
    post {
        success {
            echo '¡Pipeline ejecutado con éxito! Imagen subida a ECR y clúster actualizado vía Helm.'
        }
        failure {
            echo 'El pipeline falló. Revisa la consola de Jenkins para más detalles.'
        }
    }
}