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
            process = "./jenkins/scripts/deliver.sh".execute()
        }
        stage('Kill') {
            try {
                timeout(time: 1, unit: 'MINUTES') {
                    sh 'sleep 60'
                }
            } finally {
                if (process) {
                    echo 'Process is killed'
                    process.destroy()
                }
            }
        }
    }
}