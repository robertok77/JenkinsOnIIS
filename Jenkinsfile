pipeline {
    agent any

    environment {
        DOTNET_VERSION = '9.0' // Change this based on your .NET version
        BUILD_DIR = "c:\\Projects\\POC\\Jenkins\\WebApi"
        IIS_SITE_PATH = "C:\\inetpub\\wwwroot\\WebAPI"
        IIS_SERVICE = "W3SVC"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/robertok77/JenkinsOnIIS.git'
            }
        }

        stage('Restore Dependencies') {
            steps {
                script {
                    bat "dotnet restore"
                }
            }
        }

        stage('Build Solution') {
            steps {
                script {
                    bat "dotnet build --configuration Release"
                }
            }
        }

        stage('Publish Application') {
            steps {
                script {
                    bat "dotnet publish -c Release -o ${BUILD_DIR}"
                }
            }
        }

        stage('Deploy to IIS') {
            steps {
                script {
                    bat """
                    powershell Stop-Service ${IIS_SERVICE} -Force
                    powershell Remove-Item -Recurse -Force ${IIS_SITE_PATH}
                    powershell Copy-Item -Recurse -Force ${BUILD_DIR}/* ${IIS_SITE_PATH}
                    powershell Start-Service ${IIS_SERVICE}
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
