def registry = "https://sph.jfrog.io"
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
    }
}