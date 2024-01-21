node {
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

            sh 'heroku version'
            // // Deploy to Heroku
            // withCredentials([usernamePassword(credentialsId: 'heroku-credentials', usernameVariable: 'HEROKU_USERNAME', passwordVariable: 'HEROKU_PASSWORD')]) {
            //     sh "heroku login"
            //     sh "git push heroku HEAD:master"
            // }

            sh 'sleep 60'
        }
    }
}