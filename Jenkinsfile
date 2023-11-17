pipeline {     

    agent any

    stages {

        stage('Build step') {

            steps {
                sh '''
                ssh jenkins@long-jenkins-deploy <<EOF
                sh "sh setup.sh"
                '''
            }

        }

    }

}