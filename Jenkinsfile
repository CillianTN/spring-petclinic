pipeline {
    agent any

    tools {
        maven 'Maven 3'
    }

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

        stage('Deploy to Tomcat') {
            steps {
                script {
                    sh """
                    curl --user admin:admin -X POST http://40.71.19.193:8080/manager/text/undeploy?path=/petclinic
                    curl --upload-file target/*.war http://admin:admin@40.71.19.193:8080/manager/text/deploy?path=/petclinic
                    """
                }
            }
        }
    }

    post {
        always {
            echo "Worked sucessfully A1 Lad."
        }
    }
}
