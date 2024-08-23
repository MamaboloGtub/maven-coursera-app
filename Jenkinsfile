pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }
        stage('Package') {
            steps {
                bat 'mvn package'
            }
            post {
            always {
                junit stdioRetention: '', testResults: 'target/surefire-reports/*.xml'
            }
            success {
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
            }
        }
        }
        stage('Deploy') {
            steps {
                withEnv(['JENKINS_NODE_COOKIE=dontKillMe']){
                                bat '''
                                start /B java -jar -Dserver.port=8001 target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar
                                '''
                }
            }
        }
    }
}
