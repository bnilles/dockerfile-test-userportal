pipeline {
    agent any

    stages {
      stage('checkout') {
        steps {
          git branch: 'feature/feature_branch', credentialsId: 'git_login', url: 'https://github.com/bnilles/dockerfile-test-userportal.git'
        }
      }
        stage('Build') {
            steps {
                  
                    sh 'docker build . -t jbnilles/user_portal:latest'

                 
            }
        }
        stage("build & SonarQube analysis") {
            agent any
            steps {
              withSonarQubeEnv('SonarQube') {
                sh 'mvn clean package sonar:sonar'
              }
            }
          }
          stage("Quality Gate") {
            steps {
              
                waitForQualityGate abortPipeline: true
              
            }
          }
        stage('Deploy') {
            steps {
                sh 'docker push jbnilles/user_portal:latest'
            }
        }
    }

}