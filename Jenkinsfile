pipeline{
  agent any

  stages{
    stage("Build the Java Project"){
      steps{
        echo message: "---------Building the webapp project-----------"
        sh script: "mvn -f pom.xml clean package"
      }
      post{
        success{
          echo message: "---------Archiving the artifact-----------"
          archiveArtifacts artifacts: "**/*.war"
        }
      }
    }

    stage("Deploy to dev"){
      steps{
        azureFunctionAppPublish(
          appName: env.FUNCTION_NAME,
          azureCredentialsId: env.AZURE_CRED_ID, 
          filePath: '**/*.war,**/*.jar,bin/*,HttpTrigger-Java/*', 
          resourceGroup: env.RES_GROUP 
        ) 

        // AZURE_CRED_ID=Jenkins_CAPS_Dev
        // RES_GROUP=rg-csc-entsys-dev-caps
        // FUNCTION_NAME=fx-csc-entsys-d-caps-use

        // AZURE_CRED_ID=myServicePrincipal
        // RES_GROUP=azfn
        // FUNCTION_NAME=azfn-test

          
          // sourceDirectory: 'target/azure-functions/odd-or-even-function-sample'
      }
    }

  }
}