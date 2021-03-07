pipeline{
  agent any

  stages{
    stage("Build the Java Project"){
      steps{
        echo message: "---------------Building the webapp project------------"
        sh script: "mvn -f pom.xml clean package"
      }
      post{
        success{
          echo message: "----------Archiving the artifact------------"
          archiveArtifacts artifacts: "**/*.war"
        }
      }
    }

    stage("Deploy to staging"){
      steps{
        echo message: "---------Deploy to staging ENVIRONMENT-----------"
        build job: "webapp_Maven_Stage"
      }
    }

    stage("Deploy to prod"){
      steps{
        echo message: "------------Deploy to prod ENVIRONMENT--------------"
        timeout(time:5, unit:'DAYS'){
          input message: "Approve the deployment to Prod?"
        }
        build job: "webapp_Maven_Prod"
      }
    }
  }
}
