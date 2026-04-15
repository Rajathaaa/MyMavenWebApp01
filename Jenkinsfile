pipeline {
    agent any  

    tools {
        maven 'Maven'  
    }

    environment {
        TOMCAT_HOME = "/opt/tomcat"
        APP_NAME = "MymavenWebApp01"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Rajathaaa/MyMavenWebApp01.git'
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
                $TOMCAT_HOME/bin/shutdown.sh || true

                echo "Cleaning old deployment..."
                rm -rf $TOMCAT_HOME/webapps/$APP_NAME*
                rm -rf $TOMCAT_HOME/work/*

                echo "Deploying new WAR..."
                cp target/$APP_NAME.war $TOMCAT_HOME/webapps/

                echo "Starting Tomcat..."
                $TOMCAT_HOME/bin/startup.sh

                echo "Deployment complete!"
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
