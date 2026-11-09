pipeline {
    agent any
    options {
      skipStageAfterUnstable()
    }
    stages {
        stage('Build') { 
            steps {
                sh 'dotnet restore' 
                sh 'dotnet build --no-restore' 
            }
        }
        stage('Test') {
            steps {
                sh 'dotnet test --no-build --collect "XPlat Code Coverage"'
            }
            post {
                always {
                    recordCoverage(tools: [[parser: 'COBERTURA', pattern: '**/*.xml']], sourceDirectories: [[path: 'SimpleWebApi.Test/TestResults']])
                }
            }
        }
        stage('Deliver') {
            steps {
                sh 'dotnet publish SimpleWebApi --no-restore  -o published' 
            }
            post {
              success {
                    archiveArtifacts artifacts: 'published/**', fingerprint: true
                }
            }
        }

    }
}