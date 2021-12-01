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
     stage ('deploy')
     {
         steps
         {
             sh 'cp -R /root/.jenkins/workspace/task/target/* /opt/apache-tomcat-8.5.3/webapps'
         }
     }
       stage ('increase versions')
        {
            steps
            {
             sh 'mvn -U versions:set -DnewVersion=${version}'
        }
    }
  
        stage('Slack it'){
            steps {
                slackSend channel: '#developer', 
                          message: 'Hello, world'
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
