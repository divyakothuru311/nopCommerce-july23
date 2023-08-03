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
                    id: "DOTNET_RESOLVER",
                    serverId: "jfrogconnection",
                    repo: "nop-nuget-remote"
                )
            }
        }

        stage ('Exec build') {
            steps {
                rtDotnetRun (
                    resolverId: "DOTNET_RESOLVER",
                    args: "build src/NopCommerce.sln"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "jfrogconnection"
                )
           }
        
        }
    }
}  