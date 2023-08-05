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

        // stage('build and package') {
        //     steps {
        //         rtDotnetResolver(
        //             id: "devops",
        //             serverId: "instance",
        //             repo: "nopy-nuget-local"
        //         )
        //     }
        // }

        // stage ('build') {
        //     steps {
        //         rtDotnetRun (
        //             resolverId: "devops",
        //             args: "build src/NopCommerce.sln"
        //         )
        //     }
        // }

        // stage ('Publish build info') {
        //     steps {
        //         rtPublishBuildInfo (
        //             serverId: "instance"
        //         )
        //    }
        
        // }

        stage('sonar scanner') {
            steps {
                def sqScannerMsBuildHome = tool 'devops-commerce'
                withSonarQubeEnv('devops-commerce') {
                
                  //sh 'dotnet tool install --global dotnet-sonarscanner'
                  sh 'dotnet sonarscanner begin k:"divyakothuru311_devops-commerce" o:"divyakothuru311" d:sonar.token="cf2551d07e163cc0b29bfb7c45b1c02a5147a57c"'
                  sh 'dotnet build /home/ubuntu/jenkins_home/workspace/devops/src/NopCommerce.sln'
                  sh 'sudo /home/ubuntu/.dotnet/tools/dotnet-sonarscanner end'

                }  
            }
        
        }
    }  
}        