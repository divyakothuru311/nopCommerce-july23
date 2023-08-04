pipeline{
    agent { label 'dotnet'}
    
    triggers {
        pollSCM('* * * * *') 
    }
    stages {
        stage('git vcs') {
            steps {
                git url: 'https://github.com/divyakothuru311/nopCommerce-july23.git',
                    branch: 'develop'
            }
        }

        stage('build and package') {
            steps {
                rtDotnetResolver(
                    id: "devops",
                    serverId: "instance",
                    repo: "nopy-nuget-local"
                )
            }
        }

        stage ('build') {
            steps {
                rtDotnetRun (
                    resolverId: "devops",
                    args: "build src/NopCommerce.sln"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "instance"
                )
           }
        
        }

        stage('sonar scanner') {
            steps {
                withSonarQubeEnv('Sonarqube') {
                  sh 'dotnet tool install --global dotnet-sonarscanner'
                  sh 'dotnet sonarscanner begin /k:"divyakothuru311_devops-commerce" /o:"divyakothuru311" /d:sonar.token="5fc92ed3504db030d05f2d4ebacb5cb12a30385f"'
                  sh 'dotnet build nopCommerce-july23\src\Presentation\Nop.Web\Nop.Web.csproj',
                  sh 'dotnet sonarscanner end /d:sonar.token="5fc92ed3504db030d05f2d4ebacb5cb12a30385f"'

                }  
            }
        
        }
    }  
}    