currentBuild.displayName = "Artifactory - #"+currentBuild.number
pipeline {
  agent any
   stages {
      stage("CheckOut-SCM"){
          steps{
                      cleanWs()
                      checkout([$class: 'GitSCM',
                                branches: [[name: '*/master']]
                                ,doGenerateSubmoduleConfigurations: false,
                                extensions: [],
                                submoduleCfg: [],
                                userRemoteConfigs: [[credentialsId: '861a346a-a5a0-4d80-91f5-610cb23f2710',
                                                     url: 'https://github.com/kishore31b/Isolvers.git']]])
                }
                     }
       stage("Build"){
           steps{
                    sh label: '', script: 'mvn package'
                   }
             }
             
             stage("archiveArtifacts"){
           steps{
                   archiveArtifacts '**/*.war'
                   }
             }
            
            stage("Deploying"){
                steps{
                  sh label: '', script: 'scp /var/lib/jenkins/workspace/pipline/webapp/target/webapp.war ubuntu@15.206.127.63://var/lib/tomcat8/webapps/harindra.war'
                }
            }
            
            stage ("Testing"){
                steps{
                    checkout([$class: 'GitSCM', 
                              branches: [[name: '*/master']], 
                              doGenerateSubmoduleConfigurations: false, 
                              extensions: [],
                              submoduleCfg: [], 
                               userRemoteConfigs: [[credentialsId: '861a346a-a5a0-4d80-91f5-610cb23f2710', 
                                                    url: 'https://github.com/kishore31b/Testing.git']]])
                   sh 'java -jar  /var/lib/jenkins/workspace/pipline/testing.jar'
                }
            }
        stage('upload') {
           steps {
              script { 
                 def server = Artifactory.server 'Artifactory-1'
                 def uploadSpec = """{ 
                   "files": [{
                       "pattern": "**/*.war",
                       "target": "maven-repo-artifactory"
                       
                    }]
                 }"""
                 
                 server.upload(uploadSpec) 
            }
       }
     }
  }
            post {
        success {  // notify users when the Pipeline is successful
            mail(to: 'harireddy090@gmail.com', 
                 subject: "Pipeline Complete: ${currentBuild.fullDisplayName}",
                 body: "Was successfully deployed. ${env.BUILD_URL}")
        }
        failure {  // notify users when the Pipeline fails
            mail(to: 'harireddy090@gmail.com', 
                 subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                 body: "Something is wrong with ${env.BUILD_URL}")  
        }
     
     }        
                   
 }
