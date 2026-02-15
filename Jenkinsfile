pipeline {
    agent any

    tools {
        maven "M3"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/AvikCodeCrafter/Java-Monitoring-Project.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }

            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }

        stage('Deploy Standalone App') {
            steps {
                sh '''
                    echo "Stopping old application if running..."
                    PID=$(lsof -t -i:8080)
                    kill -9 $PID || true

                    echo "Starting new application..."
                    nohup java -jar target/*.jar > app.log 2>&1 &
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
