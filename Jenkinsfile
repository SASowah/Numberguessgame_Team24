pipeline {
    agent any

    environment {
        PROJECT_NAME = "NumberGuessGame"
        ARTIFACT = "target/NumberGuessGame-1.0-SNAPSHOT.war"
        SERVER_IP = "13.60.241.144"  /* Replace with your actual Tomcat Server IP */
        TOMCAT_URL = "http://${SERVER_IP}:8080/manager/text"  // Tomcat Manager URL
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
                        curl -v --user $TOMCAT_USER:$TOMCAT_PASSWORD --upload-file ${ARTIFACT} ${TOMCAT_URL}/deploy?path=/${PROJECT_NAME}
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
