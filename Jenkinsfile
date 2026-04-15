pipeline {
    agent any  

    tools {
        maven 'Maven'  
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Hemanth-bs/MyMavenWebApp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "Stopping Tomcat..."
                sudo systemctl stop tomcat || true

                echo "Removing old deployment..."
                rm -rf /opt/tomcat/webapps/MyMavenWebApp*
                
                echo "Deploying new WAR..."
                cp target/*.war /opt/tomcat/webapps/

                echo "Starting Tomcat..."
                sudo systemctl start tomcat
                '''
            }
        }    
    }

    post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
