pipeline {
    agent any
    stages {
        stage('opdracht 5') {
            steps {
                echo "good luck..."
            }
        }
        stage('checkout') {
            steps {
                git branch: 'main', credentialsId: 'Github', url: 'https://github.com/MetehanKarakorukPXL/calculator-app-finished'                
                sh 'ls -lah'
            }
        }
        stage('Install dependencies') {
            steps {
                nodejs('nodetin') {
                    sh 'npm install'
                }
            }
        }
        stage('Unittests'){
            steps{
                nodejs('nodetin') {
                    sh 'npm test'
                }
                junit keepProperties: true, testResults: 'junit.xml'
            }
        }
        stage('create bundle'){
            steps{
                sh 'mkdir bundle ; cp -r app.js docker-compose.yml Dockerfile .dockerignore junit.xml node_modules package.json package-lock.json public routes.js server.js ./bundle/ ; ls -lah bundle ; zip bundle.zip bundle'
            }
        }
    }
    post {
        success {
            echo "Pipeline poging successful"
            archiveArtifacts artifacts: 'bundle.zip', followSymlinks: false
        }
        failure {
            echo "Pipeline poging gefaald op ${currentBuild.getTime()}"
            sh 'echo "Pipeline poging faalt op ${currentBuild.getTime()}" >> /var/lib/jenkins/jenkinserrorlog'
        }
        cleanup {
            cleanWs deleteDirs: true
            echo "Post-action cleanup for entire pipeline."
            echo "Directory cleaned up SUCCESSFULLY!"
        }
    }
}
