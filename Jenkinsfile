pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Package') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
                }
            }
        }

        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                script {
                    sh 'scp ./target/spring-petclinic-*.jar admin@40.71.19.193:/opt/apps/'
                    sh 'ssh admin@40.71.19.193 "pkill -f \'java.*spring-petclinic\' ; nohup java -jar /opt/apps/spring-petclinic-*.jar > /dev/null 2>&1 &"'
                }
            }
        }
    }

    post {
        always {
            echo "Build and Deployment process completed with status: ${currentBuild.result}"
        }
    }
}
