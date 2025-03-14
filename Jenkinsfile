pipeline {
    agent any

    environment {
        PROJECT_NAME = "NumberGuessGame"
        ARTIFACT = "target/NumberGuessGame-2.0-SNAPSHOT.war"
        DEPLOY_DIR = "/home/ec2-user/apache-tomcat-7.0.94/webapps"
        TOMCAT_USER = "ec2-user"
        SERVER_IP = "13.60.241.144"  /* Replace with your actual Tomcat Server IP */
        TOMCAT_URL = "http://${SERVER_IP}:8080/manager/text"  // Define Tomcat Manager URL
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                 git branch: 'main', url: 'https://github.com/saakanbi/Numberguessgame_Team24.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'tomcat-credentials', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASSWORD')]) {
                        sh """
                        scp ${ARTIFACT} ${TOMCAT_USER}@${SERVER_IP}:${DEPLOY_DIR}/NumberGuessGame.war
                        ssh ${TOMCAT_USER}@${SERVER_IP} "sudo systemctl restart tomcat"
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Build or deployment failed!'
        }
    }
}
