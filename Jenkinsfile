@Library('JenkinsLibrary') _
def readPropertiesValues(){
 def props = readProperties file: 'myvariables.properties'
 return props;
 }
import groovy.json.*

pipeline
{
agent any
  
  stages {
     stage ('Reading variables') {
     steps {
        script {
                props = readPropertiesValues()
                println props.getClass()
     }
    }
  }
   stage('checkout'){
     steps {
       //CheckoutWorkspace(branch: 'master', scmUrl: 'https://github.com/DineshNataraj/spring-framework-petclinic.git')
       echo "checkout workspace print"
       checkout(branch: props["branch"] , scmUrl: props["repo"]) 
       // myDeliveryPipeline('master', 'https://github.com/DineshNataraj/spring-framework-petclinic')
     }
    }
    stage('mvn build')
    {
      steps {
        mavenbuild()
      }
    }
    stage('Junit Testing') {
       steps {
          testing(props["testpath"])
       }
    }
