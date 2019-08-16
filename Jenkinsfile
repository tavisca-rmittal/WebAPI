pipeline{
    agent { label 'master' }
    parameters{
        string(
            name: "GIT_PATH",
            defaultValue: "https://github.com/tavisca-rmittal/WebApi.git",
            description: "GIT HTTPS PATH"
        )
        string(
            name: "SOLUTION_PATH",
            defaultValue: "HiHelloApi.sln",
            description: "SOLUTION_PATH"
        )
        string(
            name: "DOTNETCORE_VERSION",
            defaultValue: "2.2",
            description: "Version"
        )
        string(
            name: "TEST_SOLUTION_PATH",
            defaultValue: "HiHelloApi.Tests/HiHelloApi.Tests.csproj",
            description: "TEST SOLUTION PATH"
        )
        
        string(                                             
            name: "PROJECT_PATH",
            defaultValue: "HiHelloApi.Tests/HiHelloApi.Tests.csproj",
            description: "TEST SOLUTION PATH"
        )
        choice(
            name: "RELEASE_ENVIRONMENT",
            choices: ["Build","Test", "Publish"],
            description: "Tick what you want to do"
        )
    }
    stages{
        stage('Build'){
            when{
                expression{params.RELEASE_ENVIRONMENT == "Build" || params.RELEASE_ENVIRONMENT == "Test" || params.RELEASE_ENVIRONMENT == "Publish"}
            }
            steps{
                sh '''
                    echo '====================Build Project Start ================'
                    dotnet restore ${SOLUTION_PATH} --source https://api.nuget.org/v3/index.json
                    echo '=====================Build Project Completed============'
                    echo '====================Build Project Start ================'
                    dotnet build ${PROJECT_PATH} 
                    echo '=====================Build Project Completed============'
                '''
            }
        }
        stage('Test'){
            when{
                expression{params.RELEASE_ENVIRONMENT == "Test" || params.RELEASE_ENVIRONMENT == "Publish"}
            }
            steps{
                powershell '''
                    echo '====================Build Project Start ================'
                    dotnet test ${TEST_SOLUTION_PATH}
                    echo '=====================Build Project Completed============'
                '''
            }
        }
        stage('Publish'){
            when{
                expression{params.RELEASE_ENVIRONMENT == "Publish"}
            }
            steps{
                powershell '''
                    echo '====================Build Project Start ================'
                    dotnet publish ${PROJECT_PATH}
                    echo '=====================Build Project Completed============'
                '''
            }
        }
        stage ('push artifact') {
            when{
                expression{params.RELEASE_ENVIRONMENT == "Publish"}
            }
            steps {
                powershells '''
                zip zipFile: 'publish.zip', archive: false, dir: 'WebApi/HiHelloApi/bin/Debug/netcoreapp2.2/publish'
                archiveArtifacts artifacts: 'publish.zip', fingerprint: true
                '''
            }
        }
    }
    post{
        always{
            deleteDir()
       }
    }
}