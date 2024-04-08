pipeline {
    agent any
    tools{
        maven 'maven 3.9'
        jdk 'java17'
    }
    stages {
        stage('download code ')  {
            steps{
                echo " downloading code from github"
                git branch: 'main', url: 'https://github.com/skagath/jenkins2-maven.git'
            }
        }
        stage('build code ') {
            steps{
                echo " building the code"
                sh 'mvn clean package'
            }
        }
        stage('unit-test ') {
            steps{
                echo " unit testing"
                junit stdioRetention: '', testResults: '**/target/surefire-reports/*.xml'
            }
        }
        stage('archive artifect '){
            steps{
                echo " archaving the artifact"
                archiveArtifacts artifacts: '**/*.war', followSymlinks: false
            }
        }
        stage('deploy '){
            steps{
                echo " deploying to container"
                deploy adapters: [tomcat9(credentialsId: 'tomcatdevcred', path: '', url: 'http://34.125.94.39:8090/')], contextPath: null, war: '**/*.war'
            }
        }
    }
}
