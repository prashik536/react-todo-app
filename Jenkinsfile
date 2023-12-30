pipeline {
    agent any

    stages {
        stage('Stop Deployment') {
            steps {
                script {
                    // Check if the process exists before stopping it
                    def processExists = sh(script: 'pm2 id react-todo-app', returnStatus: true) == 0
                    if (processExists) {
                        sh 'pm2 stop react-todo-app'
                    } else {
                        echo 'No existing process found. Skipping stop.'
                    }
                }
            }
        }

        stage('Pull Fresh Code') {
            steps {
                script {
                    // Pull fresh code from the updated Git repository
                    sh 'git clone https://github.com/prashik536/react-todo-app.git /opt/checkout/react-todo-add'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the code to the deployment directory
                    sh 'npm install --prefix /opt/checkout/react-todo-add'
                    sh 'npm run build --prefix /opt/checkout/react-todo-add'
                }
            }
        }

        stage('Deploy using PM2') {
            steps {
                script {
                    // Deploy using PM2
                    sh 'pm2 start /opt/checkout/react-todo-add/build/app.js --name react-todo-app'
                }
            }
        }

        stage('Upload to S3') {
            steps {
                script {
                    // Upload build to S3 using the shared S3 credentials
                    s3Upload(
                        credentialsId: 'your-s3-credentials-id',
                        files: 'dist/**',  // Adjust the path based on your build output
                        bucket: 'your-s3-bucket',
                        path: 'optional-path-in-bucket'
