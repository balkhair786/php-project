pipeline {
    agent any
    stages{
        stage('Build Maven'){
            steps{
                git url:'https://github.com/balkhair786/php-project/', branch: "master"
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t balkhair786/phpproject:v1 .'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push balkhair786/phpprojecthbaimg:v1'
                }
            }
        }
        
     stage('Deploy') {
            steps {
               script {
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe21 -p 8081:80 balkhair786/phpproject:v1'
                    sshagent(['hba-key']) {
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe211 -p 8082:80 balkhair786/phpproject:v1"
                         sh "ssh -o StrictHostKeyChecking=no paperlive-u2204-docker-master@192.168.56.102 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
