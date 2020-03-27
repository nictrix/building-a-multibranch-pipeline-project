pipeline {
    agent any
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                script {
                  sh(script: '#!/usr/bin/env bash; source ~/.bash_profile; rbenv local 2.5.0; gem install bundler; bundle install')
                }
            }
        }
        stage('Test') {
            steps {
                sh 'bundle exec rake test'
            }
        }
        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                sh 'bundle exec rake db:migrate; bundle exec rackup --host 0.0.0.0'
                input message: 'Click "Proceed" to continue'
                sh 'echo "deleting!"'
            }
        }
        stage('Deploy for production') {
            when {
                branch 'production'
            }
            steps {
                sh 'bundle exec rake db:migrate; bundle exec rackup --host 0.0.0.0'
                input message: 'Click "Proceed" to continue'
                sh 'echo "deployed!"'
            }
        }
    }
}