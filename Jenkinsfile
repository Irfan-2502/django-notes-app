pipeline {
    agent any

    environment {
        PROJECT_ID = 'project-5fb420c3-a64f-40e2-906'
        REGION = 'us-central1'
        REPOSITORY = 'zango'
        IMAGE_NAME = 'notes-app'
    }

    stages {

        stage('Cloning the code') {
            steps {
                echo "This stage is for cloning the code"

                git branch: 'main',
                    url: 'https://github.com/LondheShubham153/django-notes-app.git'

                echo "Code cloned successfully"
            }
        }

        stage('Build the Docker image') {
            steps {
                echo "Building the Docker image"

                sh '''
                    docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Push image to Artifact Registry') {
            steps {
                echo "Pushing Docker image to Artifact Registry"

                sh '''
                    gcloud auth configure-docker $REGION-docker.pkg.dev

                    docker tag $IMAGE_NAME \
                    $REGION-docker.pkg.dev/$PROJECT_ID/$REPOSITORY/$IMAGE_NAME:$BUILD_NUMBER

                    docker push \
                    $REGION-docker.pkg.dev/$PROJECT_ID/$REPOSITORY/$IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }

        stage('Remove Docker image') {
            steps {
                echo "Removing Docker image from Jenkins server"

                sh '''
                    docker rmi $IMAGE_NAME
                '''
            }
        }
    }
}
