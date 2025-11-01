pipeline{
    agent any
    environment{
        IMAGE_NAME= "babugudageri/django-ecom"
    }
    stages{
        stage('git-checkout'){
            steps{
                echo "cloning the repository"
                git branch: "main", git url: "https://github.com/BasavarajGudageri-05/Django-ecommerce-application.git"
            }
        }
        stage('Docker build'){
            steps{
                echo "build docker image"
                sh "docker built -t $IMAGE_NAME:latest ."
            }
        }
        stage('push to Docker'){
            steps{
                echo "pushing image to docker hub"
                withCredentials([usernamePassword(credentialsId: 'credentialsid', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USER')]) {
               
               sh """
                echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USER" --password-stdin 
                docker push $IMAGE_NAME:latest
                 """
}

              

            }
              
        }
        stage('Deploying to Kubernates'){
            steps{
                echo "deploying to kubernates"
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'kubecredID', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                sh """
                 echo "applying Deployment"
                 kubectl apply -f Deployment.yml
                 echo "applying Service"
                 kubectl apply -f Service.yml
                """
}
            }
        }
    }
}

    post {
        success {
            echo "✅ Kubernetes deployment successful"
        }
        failure {
            echo "❌ Deployment failed, check Jenkins logs"
        }
    }
