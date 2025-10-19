pipeline {
    agent any

    environment {
        // Use Jenkins credentials IDs
        TOMCAT_USER = credentials('tomcat-creds')
        GITHUB_TOKEN = credentials('github-creds1')
    }

    stages {
        stage('Checkout Source') {
            steps {
                echo "📦 Checking out InsureMe source code..."
                git branch: 'main',
                    url: 'https://github.com/chandrukandagallu/InsureMe-20Mar.git',
                    credentialsId: 'github-creds1'
            }
        }

        stage('Build with Maven') {
            steps {
                echo "🔧 Building the application with Maven..."
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to Tomcat (8081)') {
            steps {
                echo "🚀 Deploying application to Tomcat..."
                sh """
                    # Copy JAR to Tomcat webapps or appropriate deployment directory
                    sudo cp target/insure-me-1.0.jar /opt/tomcat/webapps/
                    
                    # Restart Tomcat server
                    sudo systemctl restart tomcat
                """
            }
        }
    }

    post {
        success {
            echo "✅ Build and deployment successful!"
        }
        failure {
            echo "❌ Build or deployment failed! Check logs."
        }
    }
}
