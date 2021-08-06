pipeline{
    agent any
    
    environment {
        dotnet ='/usr/bin/dotnet'
        }
        
    triggers {
        pollSCM 'H * * * *'
    }
    stages{
        stage ('Clean workspace') {
            steps {
                cleanWs()
            }
        }        
        stage('Checkout') {
            steps {
                git url: 'https://github.com/dotnet-architecture/eShopOnWeb'
                }
        }
        stage('Restore packages'){
            steps{
                sh "dotnet restore src/Web/Web.csproj"
                }
        }
        stage('Clean'){
            steps{
                sh "dotnet clean src/Web/Web.csproj"
                }
        }
        stage('Build'){
            steps{
                bat "dotnet src/Web/Web.csproj -c Release"
                }
        }
        stage('Test: Unit Test'){
            steps{
                sh "dotnet test tests/UnitTests/UnitTests.csproj"
                }
        }
        stage('Test: Integration Test'){
            steps{
                sh "dotnet test tests/IntegrationTests/IntegrationTests.csproj"
                }
        }        
        stage('Publish'){
            steps{
                sh "dotnet publish src/Web/Web.csproj"
                }
        }                            
    }
    post{
        always{
            emailext body: "${currentBuild.currentResult}: Job   ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
            recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
            subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
        }
    }    
 }