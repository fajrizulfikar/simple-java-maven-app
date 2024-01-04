node {
    docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
        stage('Build') {
            sh 'mvn -B -DskipTests clean package'
        }
        stage('Test') {
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
        }
        stage('Deploy') {
            def process = "./jenkins/scripts/deliver.sh".execute()

            process.waitFor()

            timeout(time: 1, unit: 'MINUTES') {
                currentBuild.result = 'ABORTED'
            }
        }
    }
}