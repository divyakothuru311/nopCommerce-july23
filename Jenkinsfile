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
    }
}  