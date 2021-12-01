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
   }
}
