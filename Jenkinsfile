pipeline {
    agent any
    tools {
        maven 'Maven'
        jdk 'jdk8221'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh "mvn clean install"
            }
        }
    }
    stage('Share artifact') {
        agent any
        stages {
            stage('Test') {
                steps {
                    echo 'Testing..'
                    sh "java -jar jenkins-tests-1.0-SNAPSHOT.jar"

                }
            }
            stage('Deploy') {
                steps {
                    echo 'Deploying....'
                }
            }
        }
    }
}