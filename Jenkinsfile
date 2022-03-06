node{
    stage("CheckOutCode")
    {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sivakethineni/CI-CD-project.git']]])
    }
    
    stage("Build")
    {
        sh 'mvn install'
    }
    
    stage("SonarQubeReport")
    {
        withSonarQubeEnv(credentialsId: '72368cb1-a59e-4cb5-8666-e1982713bfe5') {
             sh 'mvn sonar:sonar'
      }
    }
    
    stage("SonarQualityGateCheck"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
              else {
                  print "Pipeline success: ${qg.status}"
              }
          }
      }
    
    stage("ArtifactUploader"){
      nexusArtifactUploader artifacts: [[artifactId: '$BUILD_TIMESTAMP', classifier: '', file: 'target/vprofile-v1.war', type: 'war']], credentialsId: 'f2bb98d3-de49-4427-bbea-ffdebda36c55', groupId: 'Dev', nexusUrl: 'http://172.31.92.39:8081/', nexusVersion: 'nexus2', protocol: 'http', repository: 'VProfile-repo', version: '$BUILD_ID'
    }
    stage("TomcatDeploy-QA"){
        deploy adapters: [tomcat8(credentialsId: '64fe9b87-b934-4f46-bfdb-e1f6e51c9c5a', path: '', url: 'http://44.202.44.125:8080/')], contextPath: null, war: '**/*.war' 
    }
    stage("TomcatDeploy-PROD"){
        deploy adapters: [tomcat8(credentialsId: '64fe9b87-b934-4f46-bfdb-e1f6e51c9c5a', path: '', url: 'http://54.86.118.1:8080/')], contextPath: null, war: '**/*.war'  
    }
}
