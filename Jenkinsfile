pipeline {
    agent any

    stages {
         stage('Set Permissions') {
            steps {
                script {
                    // Execute the script to change permissions
                    sh 'sudo /opt/scripts/change_permissions.sh'
                }
            }
        }
        stage('Pull Fresh Code') {
    steps {
        script {
            def repoDir = '/opt/checkout/react-todo-add'
            
            // Check if the directory exists
            if (fileExists(repoDir)) {
                // If it exists, pull changes
                sh "git -C ${repoDir} pull origin master"
            } else {
                // If it doesn't exist, clone the repository
                sh "git clone https://github.com/prashik536/react-todo-app.git ${repoDir}"
            }
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
            def appPath = '/opt/checkout/react-todo-add/build/static/js/main.e9d6ff45.js'
            sh "pm2 start ${appPath} --name react-todo-app"
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
