pipeline {
  agent any


  tools {
      nodejs "node"
    }


  environment {
        EMAIL_RECIPIENTS = 'gishandesilva915@gmail.com'
        SNYK_TOKEN = credentials('SNYK_TOKEN')
    }


  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Gishan-ui/8.2CDevSecOps.git'
            }
          }


        stage('Install Dependencies') {
          steps {
            sh 'npm install'
            }
          }


        stage('Run Tests') {
          steps {
            script {
                    def testStatus = 'SUCCESS'
                    try {
                      sh 'npm test' // Allows pipeline to continue despite test failures
                      }
                      catch (err) {
                        testStatus = 'FAILURE'
                        currentBuild.result = 'FAILURE'
                    } finally {
                        emailext(
                            subject: "Jenkins: Test Stage - ${testStatus}",
                            body: """Hello,


                The Test stage of the Jenkins pipeline has completed.

                Result: ${testStatus}
                Job: ${env.JOB_NAME}
                Build: ${env.BUILD_NUMBER}
                URL: ${env.BUILD_URL}
                
                Regards,
                Jenkins
                """,
                            to: "${EMAIL_RECIPIENTS}",
                            attachLog: true
                        )
                    }
                }
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
                script {
                    def auditStatus = 'SUCCESS'
                    try {
                        sh 'npm audit'
                    } catch (err) {
                        auditStatus = 'FAILURE'
                        currentBuild.result = 'FAILURE'
                    } finally {
                        emailext(
                            subject: "Jenkins: Security Scan - ${auditStatus}",
                            body: """Hello,

The Security Scan (npm audit) stage of the Jenkins pipeline has completed.

Result: ${auditStatus}
Job: ${env.JOB_NAME}
Build: ${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}

Regards,
Jenkins
""",
                            to: "${EMAIL_RECIPIENTS}",
                            attachLog: true
                        )
                    }
                }
            }
        }
    }
}
