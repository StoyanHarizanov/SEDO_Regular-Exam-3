pipeline {
    agent any

    triggers {
        // GitHub push trigger (needs webhook setup)
        githubPush()
    }

    environment {
        // Настройка на .NET версия (можеш да смениш версията)
        DOTNET_VERSION = '7.0'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup .NET') {
            steps {
                // Инсталиране/използване на нужната .NET SDK
                // Ако агентът вече има .NET, можеш да пропуснеш
                echo "Using .NET version ${env.DOTNET_VERSION}"
            }
        }

        stage('Restore Dependencies') {
            steps {
                sh "dotnet restore"
            }
        }

        stage('Build') {
            steps {
                sh "dotnet build --configuration Release --no-restore"
            }
        }

        stage('Run Tests') {
            steps {
                sh "dotnet test --no-build --verbosity normal"
            }
        }
    }

    post {
        always {
            junit '**/TestResults/*.xml' // събира тестовите резултати (ако използваш xUnit/NUnit)
        }
        failure {
            echo 'Build or tests failed!'
        }
        success {
            echo 'Build and tests succeeded!'
        }
    }
}
