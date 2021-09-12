pipeline {
    agent any

    stages {
      stage('checkout') {
        steps {
          git branch: 'feature/feature_branch', credentialsId: 'git_login', url: 'https://github.com/bnilles/dockerfile-test-userportal.git'
        }
      }
        
        stage("build & SonarQube analysis") {
            agent any
            steps {
              withSonarQubeEnv('SonarQube') {
                sh 'npm install'
                sh 'npm install sonarqube-scanner --save-dev'
                sh 'npm run sonar'
              }
            }
          }
          stage('Build') {
            steps {
                  
                    sh 'docker build . -t jbnilles/user_portal:latest'

                 
            }
        }
          
        stage('Deploy') {
            steps {
                sh 'docker push jbnilles/user_portal:latest'
            }
        }
    }

}