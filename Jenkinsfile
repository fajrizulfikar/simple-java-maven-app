node {
    docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
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
            sh 'git push heroku master'

            sh 'sleep 60'
        }
    }
}