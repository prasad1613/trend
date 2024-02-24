def registry = "https://sph.jfrog.io"
def imageName = "sph.jfrog.io/docker-trial/ttrend"
def version = "2.1.6"
pipeline {
    agent {
        node {
            label 'maven'
        }
    }
    
environment {
    PATH = "/opt/maven/bin:$PATH"
}
    stages {
        stage('build') {
            steps {
                echo "---------------- build start -------------"
                sh "mvn clean install deploy -Dmaven.test.skip=true"
                echo "----------------- build end -------------"
            }
        }
        stage('test') {
            steps {
                echo "--------------------- test start -------------"
                sh "mvn surefire-report:report"
                echo "--------------------- test end --------------"
            }
        }
         stage("Jar Publish") {
          steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"jfrog"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "libs-release-local/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
            }
        }   
    }
         stage(" Docker Build ") {
           steps {
            script {
              echo '<--------------- Docker Build Started --------------->'
              app = docker.build(imageName+":"+version)
              echo '<--------------- Docker Build Ends --------------->'
        }
      }
    }

         stage (" Docker Publish "){
             steps {
               script {
                 echo '<--------------- Docker Publish Started --------------->'  
                 docker.withRegistry(registry, 'jfrog'){
                    app.push()
                }    
                echo '<--------------- Docker Publish Ended --------------->'  
            }
        }
    }       
    }
}