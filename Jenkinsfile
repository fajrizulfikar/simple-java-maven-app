node {
    stage('Checkout') {
        checkout scm
    }
    docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2 -u root:sudo') {
        stage('Build') {
            sh 'mvn clean install -DskipTests'
        }
        stage('Test') {
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
        }
        stage('Manual Approval') {
            input message: 'Lanjutkan ke tahap Deploy?'
        }
        stage('Deploy') {
            // Deploy to Heroku
            withCredentials([usernamePassword(credentialsId: 'heroku-credentials', usernameVariable: 'HEROKU_USERNAME', passwordVariable: 'HEROKU_PASSWORD')]) {
                sh "HEROKU_API_KEY=$HEROKU_PASSWORD mvn heroku:deploy"
            }

            sh 'sleep 60'
        }
    }
}