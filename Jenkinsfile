#!groovy

pipeline {
    agent any
    
    parameters {
        string(name: 'URL', defaultValue: 'src', description: 'Give the URL where files are.')
    }
    
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timestamps ()
    }
    
    stages {
        stage('Build') {
            steps {
                git branch: 'master', url: 'https://github.com/Nevest/test_repo_git.git'
            }
        }
        
        stage('Test') {
            when {
                expression { params.LOG_LEVEL == 'do_not_change' }
            }
            steps {
                bat 'robot -d results --variable url:%WORKSPACE%\\%URL% --NoStatusRC robot.robot'
            }
        }
        
        stage('Test Skipped') {
            when {
                expression { !(params.LOG_LEVEL == 'do_not_change') }
            }
            steps {
                bat 'robot -L %LOG_LEVEL% -d results --variable url:%WORKSPACE%\\%URL% --NoStatusRC robot.robot'
            }
        }
    }
    
    post {
        success {
            archiveArtifacts artifacts: 'results-mister-freeze/*'
        }
    }
}
