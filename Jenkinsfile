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
                withSonarQubeEnv('devops-commerce') {
                
                  //sh 'dotnet tool install --global dotnet-sonarscanner'
                  sh 'sudo /home/ubuntu/.dotnet/tools/dotnet-sonarscanner begin /k:"divyakothuru311_devops-commerce" /o:"divyakothuru311" /d:sonar.host.url=https://sonarcloud.io/'
                  sh 'dotnet build /home/ubuntu/jenkins_home/workspace/devops/src/NopCommerce.sln'
                  sh 'sudo /home/ubuntu/.dotnet/tools/dotnet-sonarscanner end'

                }  
            }
        
        }
    }  
}        