pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t longehdocker/api-base .
                '''
            }

        }
        stage('Push') {
            steps {
                sh '''
                docker push longehdocker/api-base
                '''
            }

        }
        stage('Deploy') {
            steps {
                sh '''
                ssh jenkins@10.154.0.32 <<EOF
                docker stop api && echo "Stopped api" || echo "api is not running"
                docker rm api && echo "removed db" || echo "api does not exist"
		docker rm -f $(docker ps -aq)
		echo "docker containers removed"
		docker pull longehdocker/api-base
		echo "docker image pulled"
                docker run -d --name api -p 80:5001 longehdocker/api-base
		echo "docker container built"
                '''
            }

        }

    }

}