pipeline {


    agent { label 'dotnet' } 

    triggers {
        pollSCM('* * * * *')

    
    }
    tools {
        jdk 'JDK_17'
         
    }

    stages { 
        stage('vcs') {
            steps {
                git url: 'https://github.com/divyakothuru311/nopCommerce-july23.git',
                    branch: 'develop'
            }
                
        }
        stage('build and package') {
           steps {
                rtDotnetResolver (
                    id : "lalitha",
                    serverId : "jfrogconnection",
                    repo : "nop-nuget-local"
                ) 
                rtDotnetRun (
                    args : "build src/NopCommerce.sln",
                    resolverId : "lalitha"
                )
                 rtPublishBuildInfo (
                    serverId: "jfrogconnection"
                 )
                 


           }
        }
         stage('results') {
            steps{
                archiveArtifacts artifacts: '**/*.dll'
            }
        }
    }
}