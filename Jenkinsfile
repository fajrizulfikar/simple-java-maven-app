node {
    environment {
        HEROKU_API_KEY = credentials('heroku-credentials')
    }
    stage('Checkout') {
        checkout scm
    }
    docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
        stage('Build') {
            sh 'mvn clean install -DskipTests'
        }
        stage('Test') {
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
        }
        stage('Deploy') {
            input message: 'Lanjutkan ke tahap Deploy?'

            // Deploy to Heroku
            withCredentials([string(credentialsId: 'heroku-credentials', variable: 'HEROKU_API_KEY')]) {
                sh "heroku auth:token:$HEROKU_API_KEY"
                sh "git push heroku HEAD:master"
            }

            sh 'sleep 60'
        }
    }
}