pipeline {
//    agent any
//    tools {
//        maven 'Maven'
//        jdk 'jdk8221'
//    }
    agent {
        kubernetes {
            label 'build-service'
            defaultContainer 'jnlp'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    job: build-service
spec:
  containers:
  - name: maven
    image: maven:3.6.0-jdk-11-slim
    command: ["cat"]
    tty: true
    volumeMounts:
    - name: repository
      mountPath: /root/.m2/repository
   volumes:
  - name: repository
    persistentVolumeClaim:
      claimName: repository
"""
        }
    }
    options {
        skipDefaultCheckout true
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                container('maven') {
                    sh "mvn clean install"
                }
                stash includes: '**/target/*.jar', name: 'artifact'
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