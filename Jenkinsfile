pipeline {
    agent any
    tools {
        maven 'Maven'
        jdk 'jdk8221'
    }
    stages {
        stage('Build') {
            agent any
            steps {
                echo 'Building..'
                sh "mvn clean install"
            }
        }
        stage {
            stages('Share artifact') {
                stage('Test') {
                    agent any
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
}