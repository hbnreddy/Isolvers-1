currentBuild.displayName = "Artifactory - #"+currentBuild.number
pipeline {
  agent any
   stages {
      stage("CheckOut-SCM"){
          steps{
                      cleanWs()
                      checkout([$class: 'GitSCM',
                                branches: [[name: '*/master']],
                                doGenerateSubmoduleConfigurations: false,
                                extensions: [],
                                submoduleCfg: [],
                                userRemoteConfigs: [[credentialsId: 'harireddy910_gitHUB',
                                url: 'https://github.com/HariReddy910/Isolvers-1.git']]])
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
            
            stage("deploying to appServer"){
           steps{
             sh label: '', script: 'scp /var/jenkins_home/workspace/Game-of-life/webapp/target/webapp.war ubuntu@172.31.26.148:/var/lib/tomcat9/webapps/app1.war'
                   }
             }
     stage('uploading artifacts to Jfrog artfactory') {
           steps {
              script { 
                 def server = Artifactory.server 'Artifactory-1'
                 def uploadSpec = """{ 
                   "files": [{
                       "pattern": "**/*.war",
                       "target": "example-repo-local"
                       
                    }]
                 }"""
                 
                 server.upload(uploadSpec) 
            }
       }
     }
      
     
     }        
                   
 }
