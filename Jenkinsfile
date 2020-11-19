pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh 'mvn jar:jar install:install help:evaluate -Dexpression=project.name'
                sh 'NAME=`mvn help:evaluate -Dexpression=project.name | grep "^[^[]"`'
                sh 'VERSION=`mvn help:evaluate -Dexpression=project.version | grep "^[^[]"`'
                sh 'java -jar target/${NAME}-${VERSION}.jar'
            }
        }
    }
}
