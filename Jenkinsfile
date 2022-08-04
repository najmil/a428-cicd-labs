pipeline {
    agent {
        docker {
            image 'node:lts-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Manual Approval') {
            steps {
                input message: 'Lanjut ke tahap Deploy?'
            }
        }

        stage('Staging') { 
            steps {
                sh './jenkins/scripts/deliver.sh'
                //input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)' 
                sleep(time: 2, unit: 'SECONDS')
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Production') {
            steps {
                sh 'curl https://cli-assets.heroku.com/install.sh | sh'
                sh 'heroku git:remote -a my-react-app123'
                sh 'git push heroku main'
            }
        }
    }
}
