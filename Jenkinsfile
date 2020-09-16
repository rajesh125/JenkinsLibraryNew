@Library('JenkinsLibraryNew') _
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
       CheckOut(branch: props["branch"] , scmUrl: props["repo"]) 
    
     }
    }
    stage('mvn build')
    {
      steps {
       withMaven(maven: 'jenkinsmaven'){
        MavenBuild()
       }
      }
    }
    stage('Testing') {
       steps {
        withMaven(maven: 'jenkinsmaven'){
          TestingJunit()
        }
       }
    }
    stage ('Build Docker') {
      steps {
        withCredentials([usernamePassword(
            credentialsId: props["dockercredid"],
            usernameVariable: "Username",
            passwordVariable: "Password"
        )]) {
        DockerBuild(props["dockhubuser"], props["dockhubrepo"], props["dockhubtag"])
        }
      }
    }
  }
}
