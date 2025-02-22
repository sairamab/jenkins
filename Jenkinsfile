pipeline {
    agent any
    tools {
        maven 'Maven-3.9.2' // Uses the Maven installed via Jenkins
    }
    stages {
        stage("cleanup") {
            steps {
                deleteDir()
            }
        }

        stage("clone repo") {
            steps {
                sh "git clone https://github.com/jenkins-docs/simple-java-maven-app.git"
            }
        }

        stage("Build") {
            steps {
                dir("simple-java-maven-app") {
                    sh "mvn clean install"
                }
            }
        }

        stage("test") {
            steps {
                dir("simple-java-maven-app") {
                    sh "mvn test"
                }
            }
        }
    }
}