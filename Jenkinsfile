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
            steps {
                bat 'robot -d results-mister-freeze --variable url:%WORKSPACE%\\%URL% --NoStatusRC robot.robot'
            }
        }
    }
    
    post {
        success {
            archiveArtifacts artifacts: 'results-mister-freeze/*'
        }
    }
}
