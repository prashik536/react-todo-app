pipeline {
    agent any

    stages {
        
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
                    s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'prashik1212', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: '', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'bucket', userMetadata: []
            }
        }
    }
}
