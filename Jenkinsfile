// ═══════════════════════════════════════════════════════════════
// RVCE Todo App — Jenkins Declarative Pipeline (Lab Demo)
// ═══════════════════════════════════════════════════════════════

pipeline {
    agent any

    stages {

        // ───────────────────────────────────────────────
        // Stage 1: Checkout Source Code
        // ───────────────────────────────────────────────
        stage('Checkout') {
            steps {
                echo '📥 Checking out source code...'
                checkout scm

                echo '📋 Repository Information:'
                echo "Workspace: ${WORKSPACE}"
            }
        }

        // ───────────────────────────────────────────────
        // Stage 2: Verify Project Structure
        // ───────────────────────────────────────────────
        stage('Verify Project Structure') {
            steps {
                script {

                    echo "🔍 Checking project files..."

                    def requiredFiles = [
                        'package.json',
                        'Dockerfile',
                        'docker-compose.yml',
                        'README.md',
                        'Jenkinsfile'
                    ]

                    for (file in requiredFiles) {
                        if (!fileExists(file)) {
                            error("Missing required file: ${file}")
                        }
                    }

                    def requiredDirs = [
                        'client',
                        'server'
                    ]

                    for (dir in requiredDirs) {
                        if (!fileExists(dir)) {
                            error("Missing required directory: ${dir}")
                        }
                    }

                    echo "✅ Project structure verified successfully."
                }
            }
        }

        // ───────────────────────────────────────────────
        // Stage 3: Install Server Dependencies
        // ───────────────────────────────────────────────
        stage('Install Server Dependencies') {
            steps {
                dir('server') {
                    script {
                        if (isUnix()) {
                            sh 'npm install'
                        } else {
                            bat 'npm install'
                        }
                    }
                }
            }
        }

        // ───────────────────────────────────────────────
        // Stage 4: Install Client Dependencies
        // ───────────────────────────────────────────────
        stage('Install Client Dependencies') {
            steps {
                dir('client') {
                    script {
                        if (isUnix()) {
                            sh 'npm install'
                        } else {
                            bat 'npm install'
                        }
                    }
                }
            }
        }

        // ───────────────────────────────────────────────
        // Stage 5: Verify Environment
        // ───────────────────────────────────────────────
        stage('Verify Environment') {
            steps {

                echo "Node Version"

                script {
                    if (isUnix()) {
                        sh 'node -v'
                        sh 'npm -v'
                        sh 'git --version'
                    } else {
                        bat 'node -v'
                        bat 'npm -v'
                        bat 'git --version'
                    }
                }

                echo "Repository Verified"
                echo "Frontend Found"
                echo "Backend Found"
                echo "Docker Configuration Found"
                echo "Jenkinsfile Found"
            }
        }

        // ───────────────────────────────────────────────
        // Stage 6: Success
        // ───────────────────────────────────────────────
        stage('Pipeline Complete') {
            steps {

                echo '''
========================================
        BUILD SUCCESSFUL
========================================

✔ GitHub Repository Connected

✔ Source Code Checked Out

✔ Project Structure Verified

✔ Dependencies Installed

✔ Environment Verified

The project is ready for Docker,
GitHub, Azure, Webhooks and further
DevOps experiments.

========================================
'''
            }
        }
    }

    post {

        success {
            echo "🎉 Pipeline executed successfully!"
        }

        failure {
            echo "❌ Pipeline failed. Check the console output."
        }

        always {
            cleanWs()
        }
    }
}