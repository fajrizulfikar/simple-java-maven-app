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
        stage('Deploy') {
            input message: 'Lanjutkan ke tahap Deploy?'

            echo "Current Directory: $PWD"
            echo "PATH: $PATH"
            echo heroku -v

            // Deploy to Heroku
            withCredentials([usernamePassword(credentialsId: 'heroku-credentials', usernameVariable: 'HEROKU_USERNAME', passwordVariable: 'HEROKU_PASSWORD')]) {
                sh 'mvn "heroku:deploy"'
            }

            sh 'sleep 60'
        }
    }
}