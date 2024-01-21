node {
    stage('Maven build') {
        docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
            sh 'mvn clean install -DskipTests'
        }
    }
    stage('Test') {
        docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
        }
    }
    stage('Build image') {
        sh 'pwd'
        dir('.') {
            sh 'docker build -t cicd-java .'
        }
    }
    stage('Deploy') {
        input message: 'Lanjutkan ke tahap Deploy?'

        // Deploy to Heroku
        sh 'heroku container:login'
        sh 'heroku container:push web -a cicd-java'
        sh 'heroku container:release web -a cicd-java'

        sh 'sleep 60'
    }
}