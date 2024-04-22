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

        stage('Ansible') {
            steps {
                // No script block needed here, as it's currently empty
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    sh """
                    curl -v -u admin:admin http://40.71.19.193:8080/manager/text/undeploy?path=/PetClinic
                    """                    
                    sh """
                    curl -v -T ./target/spring-petclinic-*.jar http://40.71.19.193:8080/manager/text/deploy?path=/PetClinic -u admin:admin
                    """
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
