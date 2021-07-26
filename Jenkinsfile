pipeline {
    agent any
    
    tools {
        maven 'M3'
    }
    
    stages {
        stage("Cleaning workspace") {
            steps{
                sh 'echo "---=--- Cleaning Stage ---=---"'
                sh 'mvn clean'
                script {
                    try {
                        sh 'docker stop myboot && docker rm myboot'
                    } catch (Exception e) {
                        sh 'echo "no container to remove"'
                    }
                }
            }
        }
        stage("Checkout") {
            steps{
                sh 'echo "---=--- Checkout ---=---"'
                git branch: 'main', url:'https://github.com/Maximedubois31/simpleBoot.git'
            }
        }
        stage("Compile") {
            steps{
                sh 'echo "---=--- Compile ---=---"'
                sh 'mvn compile'
            }
        }
        stage("Test") {
            steps{
                sh 'echo "---=--- Test ---=---"'
                sh 'mvn test'
            }
        }
        stage("Package") {
            steps{
                sh 'echo "---=--- Package ---=---"'
                sh 'mvn package -DskipTests'
            }
        }
        stage("Docker") {
            steps{
                sh 'echo "---=--- Docker ---=---"'
                sh 'docker build -t maxawsdev/myboot:1.0 .'
            }
        }
        stage("Deploy to local") {
            steps{
                sh 'echo "---=--- Deploy to local ---=---"'
                sh 'docker run -d --name myboot -p 8100:8100 maxawsdev/myboot:1.0'
            }
        }
    }
}
