pipeline {
    agent any
    environment {
        GCR_CREDENTIALS_ID = 'longgcr' // The ID you provided in Jenkins credentials
        IMAGE_NAME = 'long-build-image'
        GCR_URL = 'gcr.io/lbg-mea-15'
        PROJECT_ID = 'lbg-mea-15'
        CLUSTER_NAME = 'demo-cluster'
        LOCATION = 'europe-west2-a'
        CREDENTIALS_ID = 'ece6548c-a239-4a78-b7f2-c1512cd1d7f2'
    }

    stages {
        stage('Build and Push to GCR') {
            steps {
                script {
                    // Authenticate with Google Cloud

                    withCredentials([file(credentialsId: GCR_CREDENTIALS_ID, variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                        sh 'gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS'
                    }

                    // Configure Docker to use gcloud as a credential helper
                    sh 'gcloud auth configure-docker --quiet'

                    // Build the Docker image
                    sh "docker build -t ${GCR_URL}/${IMAGE_NAME}:latest ."

                    // Push the Docker image to GCR
                    sh "docker push ${GCR_URL}/${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Deploy to GKE') {
            steps {
                script {
                    // Deploy to GKE using Jenkins Kubernetes Engine Plugin
                    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'kubernetes/deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
                }
            }
        }
    }
}

//     stages {
//         stage('Build and Push to GCR') {
//     steps {
//         script {
//             // Authenticate with Google Cloud
//             withCredentials([file(credentialsId: GCR_CREDENTIALS_ID, variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
//                 sh 'gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS'
//             }
//             // Configure Docker to use gcloud as a credential helper
//             sh 'gcloud auth configure-docker --quiet'

//             // Build the Docker image
//             sh "docker build -t ${GCR_URL}/${IMAGE_NAME}:${BUILD_NUMBER} ."

//             // Push the Docker image to GCR
//             sh "docker push ${GCR_URL}/${IMAGE_NAME}:${BUILD_NUMBER}"

//         }
//     }
//         }
//     }
// }

// pipeline {
//     agent any
//     stages {
//         stage('Build') {
//             steps {
//                 sh '''
//                 docker build -t longehdocker/api-base .
//                 '''
//             }

//         }
//         stage('Push') {
//             steps {
//                 sh '''
//                 docker push longehdocker/api-base
//                 '''
//             }

//         }
//         stage('Deploy') {
//             steps {
//                 sh '''
//                 ssh jenkins@10.154.0.32 <<EOF
//                 docker stop api && echo "Stopped api" || echo "api is not running"
//                 docker rm api && echo "removed db" || echo "api does not exist"
// 		docker rm -f $(docker ps -aq)
// 		echo "docker containers removed"
// 		docker pull longehdocker/api-base
// 		echo "docker image pulled"
//                 docker run -d --name api -p 80:5001 longehdocker/api-base
// 		echo "docker container built"
//                 '''
//             }

//         }

//     }

// }