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
       def nextVersionFromGit(scope) 
        {
    def latestVersion = sh(returnStdout: true, script: 'git describe --tags --abbrev=0 --match *.*.* 2> /dev/null || echo 0.0.0').trim()
    def (major, minor, patch) = latestVersion.tokenize('.').collect { it.toInteger() 
         }
    def nextVersion
    switch (scope) 
            {
        case 'major':
            nextVersion = "${major + 1}.0.0"
            break
        case 'minor':
            nextVersion = "${major}.${minor + 1}.0"
            break
        case 'patch':
            nextVersion = "${major}.${minor}.${patch + 1}"
            break
    }
    nextVersion
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
