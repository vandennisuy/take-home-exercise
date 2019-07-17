pipeline {
    environment {
        registry = "continuouslee/person-api"
        registryCredential = "dockerhub"
    }
    agent any
    tools { 
        maven 'mvn361' 
        jdk 'jdk8' 
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }

        stage ('Code Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true install' 
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml' 
                }
            }
        }
        stage ('Docker Build') {
            steps {
                script {
                    docker.build (registry, "--build-arg JARFILE=person-0.0.1-SNAPSHOT.jar .")
                }
            }
        }
    }
}
