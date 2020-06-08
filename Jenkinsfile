pipeline{
    agent any
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
        localVersion = 'Testing'
        envDirectory = 'Testing'
        localSQLServer = 'DESKTOP-C2CJ47T'
        localDBName = 'GX_KB_Moka'

        msbuild = tool name: 'MSBuild'
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
    }
}