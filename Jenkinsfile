pipeline {
    agent any

    stages {
        stage('Stop Deployment') {
            steps {
                script {
                    // Stop the running deployment using PM2
                    sh 'pm2 stop react-todo-app'
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
    }
}
