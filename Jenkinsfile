pipeline {
    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git url: 'https://github.com/balkhair786/php-project/', branch: 'master'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t balkhair/phpprojectimg:v1 .'
                }
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push balkhair/phpprojectimg:v1'
                }
            }
        }

        stage('Deploy to Remote') {
            steps {
                script {
                    def dockerCmd = 'docker run -itd --name my-php-container -p 8081:80 balkhair/phpprojectimg:v1'
                    sshagent(['hba-key']) {
                        sh "ssh -o StrictHostKeyChecking=no vagrant@192.168.56.100 '${dockerCmd}'"
                    }
                }
            }
        }
    }
}
