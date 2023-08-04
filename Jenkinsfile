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

        stage('sonar scanner'){
            steps {
                withSonarQubeEnv('sonarqube') {
                    mono SonarScanner.MSBuild.exe begin \
                    /o:divyakothuru311 \
                    /k:divyakothuru311_devops-commerce \
                    /d:sonar.host.url=https://sonarcloud.io
                    dotnet build src/NopCommerce.sln
                    mono SonarScanner.MSBuild.exe end
                }
                   
            }
        
    }
}  