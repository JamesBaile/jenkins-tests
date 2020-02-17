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
                stash includes: '**/1.0-SNAPSHOT/*.jar', name: 'artifact'
            }
        }
        stage('Second stage') {
            stages('Share artifact') {
                stage('Test') {
                    agent any
                    steps {
                        echo 'Testing..'
                        unstash 'artifact'
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