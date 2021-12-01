pipeline
{
    agent any
    stages
    {
     
     stage ('compile code')
     {
         steps
         {
             sh 'mvn clean install'
         }
     }
     stage ('test')
     {
         steps
         {
             sh 'mvn test'
         }
     }
     stage ('find my binary')
     {
         steps
         {
             sh 'find / -name *.war'
         }
     }
        stage ('increase versions')
        {
            steps
            {
             sh 'mvn -U versions:set -DnewVersion=${version}'
        }
    }
     stage ('deploy')
     {
         steps
         {
             sh 'cp -R /root/.jenkins/workspace/task/target/* /opt/apache-tomcat-8.5.3/webapps'
         }
      }
        stage ('connect to s3')
         {
             steps
             {
               s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'thupilisai/sai*', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-2', showDirectlyInBrowser: false, sourceFile: '**/*.war', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'SUCCESS', profileName: 'saithupili', userMetadata: []
             }
         }
     }
     post 
    {
        always 
        {
            slackSend channel: '#developer',                
                message: "Result : ${currentBuild.currentResult}\n Job : ${env.JOB_NAME}\n buildno : ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
          }
    }
 }
