node {
    docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
        // sh 'apt-get update && apt-get install -y heroku'

        stage('Build') {
            // Maven build
            sh 'mvn -B -DskipTests clean package'

            // Archive artifacts
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        stage('Test') {
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
        }
        stage('Deploy') {
            input message: 'Lanjutkan ke tahap Deploy?'
            // Deploy to Heroku
            withCredentials([usernamePassword(credentialsId: 'heroku-credentials', usernameVariable: 'HEROKU_USERNAME', passwordVariable: 'HEROKU_API_KEY')]) {
                sh "heroku login -i"
                sh "heroku jar:deploy target/*.jar --app cicd-java"
            }

            sh 'sleep 60'
        }
    }
}