pipeline{
    agent any
    options {
        lock( resource: 'BuildMoka')
    }
    environment{
        gitCredentials = ''

        genexusId = 'GeneXus16'
        gxPath = 'C:\\Program Files (x86)\\GeneXus\\GeneXus16'
        gxserverURL = 'https://gxserver.deuxit.com'
        credId = 'GeneXusServer'
        serverKB = 'Morfy'
        serverVersion = 'Morfy'
        environment = 'Testing'
        localKbPath = 'C:\\Models\\Moka'
        localVersion = 'Moka'
        envDirectory = 'Testing'
        localSQLServer = 'DESKTOP-6VIJUBL'
        localDBName = 'GX_KB_Moka'

        msbuild = tool name: 'MSBuild'
        msbuildScript = ".\\Scripts\\Build.msbuild"
        rebuild = 'false'
        mains = 'true'

        deployWebProjName = 'MokaWeb'
        deployAppProjName = 'MokaApp'
        msdeployScript = ".\\Scripts\\Deploy.msbuild"

    }
    stages{
        stage("Update"){
            steps{
                gxserver changelog: true, poll: true,
                credentialsId: "${credId}", 
                gxInstallationId: "${genexusId}", 
                kbDbServerInstance: "${localSQLServer}",
                kbName: "${serverKB}", 
                kbVersion: "${serverVersion}", 
                localKbPath: "${localKbPath}", 
                localKbVersion: "${localVersion}", 
                serverURL: "${gxserverURL}"
            }
        }
        stage("Build"){
            steps{
                script{
                    bat label: "Build Moka Environment ${environment}",
                    script: "\"${msbuild}\\MSBuild.exe\" \"${msbuildScript}\" /t:Build /p:GX_PROGRAM_DIR=\"${gxPath}\" /p:KBPath=\"${localKbPath}\" /p:KBEnvironment=\"${environment}\" /p:KBVersion=\"${localVersion}\" /p:Rebuild=\"${rebuild}\" /p:Mains=\"${mains}\""
                }
            }
        }
        stage("DeployWeb"){
            steps{
                script{
                    bat label: "Deploy Moka Environment ${environment}",
                    script: "\"${msbuild}\\MSBuild.exe\" \"${msbuildScript}\" /t:CreateDeploy /p:GX_PROGRAM_DIR=\"${gxPath}\" /p:KBPath=\"${localKbPath}\" /p:KBEnvironment=\"${environment}\" /p:KBVersion=\"${localVersion}\" /p:ProjectName=\"${deployWebProjName}\" /p:ObjectNames=\"${deployWebProjName}\""
                }
            }
        }
        stage("DeployApp"){
            steps{
                script{
                    bat label: "Deploy Moka Environment ${environment}",
                    script: "\"${msbuild}\\MSBuild.exe\" \"${msbuildScript}\" /t:CreateDeploy /p:GX_PROGRAM_DIR=\"${gxPath}\" /p:KBPath=\"${localKbPath}\" /p:KBEnvironment=\"${environment}\" /p:KBVersion=\"${localVersion}\" /p:ProjectName=\"${deployAppProjName}\" /p:ObjectNames=\"${deployAppProjName}\""
                }
            }
        }
    }
}