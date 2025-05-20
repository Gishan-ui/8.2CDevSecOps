pipeline {
    agent any
    environment {
        RECIPIENTS = 'gishandesilva915@gmail.com' // Replace with your email
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: ' https://github.com/Gishan-ui/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || true' // Allows pipeline to continue despite test failures
            }
        }
        stage('Generate Coverage Report') {
            steps {
                // Ensure coverage report exists
                sh 'npm run coverage || true'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true' // This will show known CVEs in the output
            }
        }
    }
    post {
        always {
            emailext(
                to: "${env.RECIPIENTS}",
                subject: "Build: ${currentBuild.fullDisplayName} - ${currentBuild.currentResult}",
                body: """<p>Pipeline: ${env.JOB_NAME}</p>
                         <p>Status: ${currentBuild.currentResult}</p>
                         <p>Details: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>""",
                attachLog: true,
                mimeType: 'text/html'
            )
        }
    }
}
