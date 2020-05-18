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
                                userRemoteConfigs: [[credentialsId: '861a346a-a5a0-4d80-91f5-610cb23f2710',
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
             sh label: '', script: 'scp /var/jenkins_home/workspace/demo/webapp/target/webapp.war ubuntu@172.31.26.148:/var/lib/tomcat9/webapps/app1.war'
                   }
             }
      
     
     }        
                   
 }
