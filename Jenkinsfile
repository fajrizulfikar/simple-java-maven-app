node {
    docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
        def process

        stage('Build') {
            sh 'mvn -B -DskipTests clean package'
        }
        stage('Test') {
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
        }
        stage('Deploy') {
            input message: 'Lanjutkan ke tahap Deploy?'
            sh './jenkins/scripts/deliver.sh'
            sh 'sleep 60'
        }
    }
}